# Gulp

```sh
// init로 package.json 파일생성.-->프로젝트 생성시 무조건 선행작업
$ npm init
```

### 0. Gulp 이전 버전 제거
```sh
$ npm uninstall --global gulp # 전역 Gulp 제거
$ npm uninstall gulp          # 로컬 Gulp 제거 (프로젝트 디렉토리에서)
```

### 1. [Gulp 전역설치 및 사용할 디펜던시 설치하기]
```sh
$ npm install --global gulp  # 전역에 gulp-cli 설치, npm i -g gulp
$ npm install --save-dev gulp #--save-dev 플래가는 devDependency로써만 설치, 이옵션은 gulp와  관련 디펜던시들은 개발과정에만 필요하기 때문 gulp 플러그인 설치할때도 --save-dev 를 줘야함.

```

### 1. [Gulp v4 설치 및 시작하기]

```sh
$ npm install --global gulp-cli  # 전역에 gulp-cli 설치, npm i -g gulp-cli
```

```sh
// NPM
$ npm install --save-dev gulpjs/gulp.git#4.0  # 로컬 디렉토리에 gulp v4 설치

// Yarn
$ yarn add --dev gulpjs/gulp.git#4.0
```

#### gulpfile.js 파일 생성

```js
var gulp = require('gulp');
var concat = require('gulp-concat');

```

gulpfile 사용하기 위해서는 requrie를 사용해야함.
이러한 gulp plug-in을 사용하기 위해서는 npm으로 (--save-dev 옵션) 설치를 해줘야함.


#### 2.1 gulp.task([name,] fn)

```js
gulp.task(name, deps, func)
```

gulp.task로 gulp task를 선언, 3개의 파라미터 선언 2번째는 생략가능.
name -> task 이름 지정, 이름에 공백이 포함되면 안됨.
deps -> 현재 선언하고있는 task를 수행하기전에 먼저 실행되어야하는 task 의 배열.
func - > 실제 수행할 task의 내용을 정의하는 function.


#### 2.2 gulp.src
```js
gulp.src(files)
```
```js
gulp.src([
	'public/src/js/loginForm.js'
	'public/src/js/slider/*.js'
	'!public/src/js/slider/slider-beta.js'
	] ...);
```
* 는 모든파일
! 그파일 열외


#### 2.3 gulp.dest(path[, options])
```js
gulp.pipe(...)
```

```js
gulp.src([
	gulp.src('public/src/js/*.js')
	.pipe(stripDebug())
	.pipe(uglify())
	.pipe(concat('script.js'))
	.pipe(gulp.dest('public/dist/js'));
```
위의 예제는 stripDebug, uglify, concat 를 사용한다.
1. gulp는 public/src/js/*.js 를 가지고 옴
2. stripDug 에게 파이핑(piping), stripbug는 모든 console.log 들과 alert 를 제거.
3. uglify 로 파이핑--> javscript를 압축
4. 그후에 concat(all.js) 로 파이핑 이것을gulp.dest()로 보냄. 이 파일이 최종 output 파일임.

```js
gulp.task('default', [])
```
gulp.task('default',[]) 는  command-line에서 gulp만 실행했을때 기본값으로 사용되는 task.
특정 task를 실행하고 싶다면.

```js
gulp task-name
```
#### 3. Bulding our gulpfile

gulp-plugins | 설명

gulp-webserver  | 웹서버처럼 동작하게 하는 플러그인
gulp-concat     | js파일 병합
gulp-uglify     | js 파일 압축
gulp-sass       | sass 파일을 컴파일하기 위한 플러그인
gulp-livereload | 웹브라우저를 리로드 하기위한 플러그인

```js
npm install gulp-webserver gulp-concat gulp-uglify gulp-minify-html gulp-sass gulp-livereload --save-dev

```
한줄로 설치 가능.









