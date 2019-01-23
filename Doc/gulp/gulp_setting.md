
## npm(node Package Manager)
npm은 Node pageage Manager는 node.js에서 사용하는 모듈들을 패키지로 만들어 npm을 통해 관리하고 배포한다.
이 모듈이 사용하고있는 다른 모듈의 의존성또한 자동으로 해결.

### 0. 패키지 관리
node sass 로 사용하기 때문에 이미 node는 설치가 되어있음.
패키지 관리하는 방법만

```sh
$ npm init
```


## Gulp 설치 및 세팅 간단방법.

### 0. Gulp 이전 버전 제거
```sh
$ npm uninstall --global gulp # 전역 Gulp 제거
$ npm uninstall gulp          # 로컬 Gulp 제거 (프로젝트 디렉토리에서)
```
### 1. [Gulp v4 설치 및 시작하기]
```sh
$ npm install --global gulp-cli  # 전역에 gulp-cli 설치, npm i -g gulp-cli
$ npm install --save-dev gulpjs/gulp.git #-save-dev 플래가는 devDependency로써만 설치, 이옵션은 gulp와  관련 디펜던시들은 개발과정에만 필요하기 때문 gulp 플러그인 설치할때도 --save-dev 를 줘야함.
```


### 2. [Gulp Plugins 설치]
```sh
npm install gulp-uglify --save-dev #싱글
npm install gulp-uglify gulp-concat-css--save-dev #멀티는 나열하면 된다.
```
복사한 gulpfile.js에서  호출하는 플러그인을 모두 설치.

#### 3. Bulding our gulpfile

gulp-plugins | 설명

gulp-concat     | js파일 병합
gulp-uglify     | js 파일 압축
gulp-sass       | sass 파일을 컴파일하기 위한 플러그인


### 3. [Gulp 시작]
```sh
$ gulp
```



### 4. Gulp v4이 이전 버전과 달라진 점

아래 코드는 v4에서 새로 생긴 `gulp.parallel()` 메소드를 사용하고 있는데 이는 병렬 실행됨을 의미한다.
즉, `styles`, `scripts` 업무가 동시 수행되는 것이다. 그런데 문제는 `clean` 업무가 중복 실행되는 문제를 보여준다.
각각의 업무가 수행될 때마다 `clean` 업무가 중복실행된다는 이야기이다.

```js
gulp.task('default', gulp.parallel('styles', 'scriptes'));
gulp.task('styles', gulp.parallel('clean', ()=>{...}));
gulp.task('scripts', gulp.parallel('clean', ()=>{...}));

gulp.task('clean', ()=>{...});
```

위 코드는 다음과 같은 방법으로 문제를 해결할 수 있다. v4에서 새로 생긴 `gulp.series()` 메소드를 사용하면 업무들을
직렬적으로 관리할 수 있다. 이리 함으로서 `clean` 업무를 먼저 수행한 후, 완료가 되면 `styles`, `scripts` 업무를
병렬 수행한다.

```js
gulp.task('default',
  gulp.series('clean', gulp.parallel('styles', 'scripts'),
  ()={...});
);

gulp.task('styles', ()=>{...});
gulp.task('scripts', ()=>{...});

gulp.task('clean', ()=>{...});
```

#### Gulp v3 + run-sequence 모듈 조홥 VS Gulp v4

```js
// Gulp v3 + run-sequence
gulp.task('build', function(){
  return runSequence(
    'clean',
    ['sass', 'copy-assets'],
    'index'
  );
});

// -----------------------------------------

// Gulp v4
gulp.task('build',
  gulp.series(
    'clean',
    gulp.parallel('sass', 'copy-assets'),
    'index'
  )
);
```

-

#### 달라진 점 요약

###### sourcemaps 기본 지원

개선된 `vinyl-fs`를 통해 `gulp-sourcemaps`를 사용하지 않고도 아래와 같이 `gulp.src` 메서드를 사용할 때 옵션을 주어 소스맵을 처리할 수 있게 된다.

```js
var gulp   = require('gulp');
var minify = require('gulp-uglify');

gulp.task('uglify', function () {
  return gulp.src('js/**/*.js', {sourcemaps: true})
    .pipe(uglify())
    .pipe(gulp.dest('dist/js'));
});
```

###### 순차 혹은 동시 수행을 위한 gulp.series, gulp.parallel

Gulp v3에서는 업무를 순차적으로 수행하기 위해서 아래와 같이 의존성을 결정하는 두번째 파라메터를 통해서 처리해왔다.

```js
//가장 먼저 실행되는 task
gulp.task('first', function () {...});

// first task를 실행한 뒤에 실행하는 task들
gulp.task('second1', ['first'], function () {...});
gulp.task('second2', ['first'], function () {...});

// 가장 마지막에 실행하는 task
gulp.task('third', ['second1', 'second2'], function () {...});
```

Gulp v4 에서는 업무의 실행 순서를 보다 명시적으로 지정할 수 있도록 `gulp.series`, `gulp.parallel`가 추가된다.
위의 예를 `gulp.series`, `gulp.parallel`를 사용해서 아래와 같이 개선할 수 있다.

```js
gulp.task('third', gulp.series('first', gulp.parallel('second1', 'second2'), function () {..}));

// 이제는 아래의 task들에 더이상 dependency가 필요하지 않다.
gulp.task('second1', function () {...});
gulp.task('second2', function () {...});
gulp.task('first', function () {...});
```