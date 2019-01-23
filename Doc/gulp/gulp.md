
#### gulpfile.js 파일 생성

Gulp 모듈 로드 및 기본 업무 등록

```js
// Gulp 모듈 로드
const gulp = require('gulp');

// Gulp 기본 업무 등록
gulp.task('default', ()=>{
  console.log('Gulp 업무 시작...');
});
```

명령 프롬프트에서 `gulp` 기본 업무 실행

```sh
$ gulp

# Gulp 업무 시작...
```

### 2. [Gulp v4 API 살펴보기](https://github.com/gulpjs/gulp/blob/4.0/docs/API.md)

메소드 | 설명
--- | ---
`gulp.task`     | 수행할 업무를 정의
`gulp.src`      | 매칭되는 1개 이상의 소스 파일을 설정
`gulp.dest`     | 업무가 수행된 결과물을 출력할 위치(디렉토리) 설정
`gulp.watch`    | 파일 변경 시(지속적 관찰), 업무 처리 설정
`gulp.parallel` | 병렬 방식으로 업무 수행 설정
`gulp.series`   | 직렬 방식으로 업무 수행 설정
`gulp.symlink`  | 업무가 수행된 결과물 심볼릭 링크(symlinks) 설정
`gulp.lastRun`  | 마지막으로 업무가 실행된 시간 정보(timestamp)를 가져옴
`gulp.tree`     | 등록된 업무들의 트리를 반환
`gulp.registry` | 등록된 사용자 정의 레지스트리를 로드하여 업무에 추가 설정

-


#### 2.1 gulp.task([name,] fn)

gulp-cli, `gulp.series`, `gulp.parallel`, `gulp.lastRun`에서 실행될 업무를 등록한다.

```js
// 업무 등록 방법 1
gulp.task(function someTask() {...});

// 업무 등록 방법 2
var someTask = gulp.task('someTask');
```
#### 2.2 gulp.src

##### gulp.src(globs[, options])

globs 문법에 매칭되는 파일들을 소스로 설정한다. 이 메소드는 [스트림](http://nodejs.org/api/stream.html) 또는 [비닐](https://github.com/wearefractal/vinyl-fs) 데이터를 반환한다.

```js
gulp.src('client/templates/*.pug')
    .pipe(pug())
    .pipe(minify())
    .pipe(gulp.dest('build/minified_templates'));
```

※ [globs](https://github.com/isaacs/node-glob) 문법이란?

```js
// 1개 이상 설정할 경우 배열(Array) 문법을 사용한다.
gulp.src(['*.js', '!b*.js', 'bad.js']);
```

-

#### 2.3 gulp.dest(path[, options])

각 파이프에 설정된 업무를 마친 결과물을 산출하기 위한 디렉토리 설정이다. 필요할 경우, 2개 이상의 다른 디렉토리로 결과를 출력할 수 있다.
단, 출력되는 디렉토리는 생성되어 있어야 한다.

```js
gulp.src('./client/templates/*.pug')
    .pipe(pug())
    .pipe(gulp.dest('./build/templates'))
    .pipe(minify())
    .pipe(gulp.dest('./build/minified_templates'));
```

-

#### 2.4 gulp.watch(globs[, opts][, fn])

설정된 파일들을 관찰한 후, 변경 사항이 발생하면 등록/설정된 업무들을 진행한다.

```js
gulp.watch('js/**/*.js', gulp.parallel('concat', 'uglify'));
```

아래와 같은 방법을 사용하면 이벤트 처리도 가능하다.

```js
var watcher = gulp.watch('js/**/*.js', gulp.parallel('concat', 'uglify'));
watcher.on('change', function(path, stats) {
  console.log('파일 ' + path + '이 변경되었습니다.');
});

watcher.on('unlink', function(path) {
  console.log('파일 ' + path + '이 제거되었습니다.');
});
```

-

#### 2.5 gulp.parallel(...tasks)

병렬 처리로 등록된 업무들을 진행한다. (비동기 방식)

```js
gulp.task('one', function(done) {
  // 첫번째 처리 업무 등록
  done();
});

gulp.task('two', function(done) {
  // 두번째 처리 업무 등록
  done();
});

gulp.task('default', gulp.parallel('one', 'two', function(done) {
  // 세번째 처리 업무 등록
  done();
}));
```

-

#### 2.6 gulp.series(...tasks)

직렬 처리로 등록된 업무들을 진행한다. (동기 방식)

```js
gulp.task('one', function(done) {
  // 첫번째 처리 업무 등록
  done();
});

gulp.task('two', function(done) {
  // 두번째 처리 업무 등록
  done();
});

gulp.task('default', gulp.series('one', 'two', function(done) {
  // 세번째 처리 업무 등록
  done();
}));
```

-

#### 2.7 gulp.symlink(folder[, options])

기능적으로는 `gulp.dest` 메소드와 유사하나, 디렉토리에 결과물을 복제하는 대신 심볼릭 링크를 생성한다는 점이 다르다.

-

#### 2.8 gulp.lastRun(taskName, [timeResolution])

작업이 성공적으로 실행 된 마지막 시간(작업이 시작된 시간)의 타임 스탬프를 반환한다. 작업이 아직 실행되지 않은 경우 `undefined`를 반환한다.

```js
gulp.lastRun('someTask', 1000) // 1426000004000 반환
gulp.lastRun('someTask', 100)  // 1426000004300 반환
```

-

#### 2.9 gulp.tree(options)

등록 처리된 업무 트리를 반환한다. 예를 들어 아래와 같은 업무가 등록되어 있을 경우

```js
gulp.task('one', function(done) {
  // 첫번째 처리 업무 등록
  done();
});

gulp.task('two', function(done) {
  // 두번째 처리 업무 등록
  done();
});

gulp.task('three', function(done) {
  // 세번째 처리 업무 등록
  done();
});

gulp.task('four', gulp.series('one', 'two'));

gulp.task('five',
  gulp.series('four',
    gulp.parallel('three', function(done) {
      // 세번째 업무 처리하면서 동시적으로 콜백 함수 실행
      done();
    })
  )
);
```

아래와 같이 업무 트리를 출력한다.

```js
gulp.tree();

// output: [ 'one', 'two', 'three', 'four', 'five' ]

// ------------------------------------------------------

gulp.tree({ deep: true });

/*output: [
   {
      "label":"one",
      "type":"task",
      "nodes":[]
   },
   {
      "label":"two",
      "type":"task",
      "nodes":[]
   },
   {
      "label":"three",
      "type":"task",
      "nodes":[]
   },
   {
      "label":"four",
      "type":"task",
      "nodes":[
          {
            "label":"<series>",
            "type":"function",
            "nodes":[
               {
                  "label":"one",
                  "type":"task",
                  "nodes":[]
               },
               {
                  "label":"two",
                  "type":"task",
                  "nodes":[]
               }
            ]
         }
      ]
   },
   {
      "label":"five",
      "type":"task",
      "nodes":[
         {
            "label":"<series>",
            "type":"function",
            "nodes":[
               {
                  "label":"four",
                  "type":"task",
                  "nodes":[
                     {
                        "label":"<series>",
                        "type":"function",
                        "nodes":[
                           {
                              "label":"one",
                              "type":"task",
                              "nodes":[]
                           },
                           {
                              "label":"two",
                              "type":"task",
                              "nodes":[]
                           }
                        ]
                     }
                  ]
               },
               {
                  "label":"<parallel>",
                  "type":"function",
                  "nodes":[
                     {
                        "label":"three",
                        "type":"task",
                        "nodes":[]
                     },
                     {
                        "label":"<anonymous>",
                        "type":"function",
                        "nodes":[]
                     }
                  ]
               }
            ]
         }
      ]
   }
]
*/
```

-

#### 2.10 gulp.registry([registry])

사용자 정의 레지스트리를 정의하거나, 업무에 추가하는 설정을 한다.

gulpfile.js

```js
var gulp = require('gulp');

// myCompanyTasksRegistry.js 에 등록된 사용자 정의 레지스트리를 로드
var companyTasks = require('./myCompanyTasksRegistry.js');

// 사용자 정의 리지스트리 등록
gulp.registry(companyTasks);

gulp.task('one', gulp.parallel('someCompanyTask', function(done) {
  console.log('in task one');
  done();
}));
```

myCompanyTasksRegistry.js

```js
var util = require('util');

var DefaultRegistry = require('undertaker-registry');

// 사용자 정의 레지스트리 생성자 함수(Class) 정의
function MyCompanyTasksRegistry() {
  DefaultRegistry.call(this);
}

// DefaultRegistry 능력을 MyCompanyTasksRegistry 생성자(Class)에 상속
util.inherits(MyCompanyTasksRegistry, DefaultRegistry);

// MyCompanyTasksRegistry 생성자의 프로토타입 확장 (인스턴스 메소드 정의)
MyCompanyTasksRegistry.prototype.init = function(gulp) {
  gulp.task('clean', function(done) {
    done();
  });
  gulp.task('someCompanyTask', function(done) {
    console.log('performing some company task.');
    done();
  });
};

// 생성자(Class)로부터 생성된 인스턴스 객체 반환(외부에 공개)
module.exports = new MyCompanyTasksRegistry();
```

---

### 3. [Gulp CLI](https://github.com/gulpjs/gulp/blob/4.0/docs/CLI.md)

#### Tasks

```sh
$ gulp <task1> <task2> ...
```

#### Flags

- `-v`, `--version`            : Gulp CLI/로컬 버전 출력
- `--gulpfile <gulpfile path>` : gulpfile로 사용하고자 하는 파일 이름 설정
- `-T`, `--tasks`              : 등록된 Gulp 업무 리스트 출력
- `--tasks-simple`             : 등록된 Gulp 업무 리스트 간단하게 출력
- `--color`                    : 기본 값. Gulp 처리 결과를 컬러를 반영하여 출력
- `--no-color`                 : Gulp 처리 결과를 컬러를 반영하지 않고 출력
- `-S`, `--silent`             : 모든 Gulp 기록(Logging)을 중지

---

### 4. 참고 자료

- [gulpjs.com](http://gulpjs.com/)
- [GULP 입문] (https://programmingsummaries.tistory.com/356)
- [Gulp v4, Github](https://github.com/gulpjs/gulp/tree/4.0/docs)
- [Gulp 4: The new task execution system - gulp.parallel and gulp.series](https://fettblog.eu/gulp-4-parallel-and-series/)
- [Migrating to gulp 4 by example](https://blog.wearewizards.io/migrating-to-gulp-4-by-example?utm_campaign=chrome_series_introtogulp_031616&utm_source=chromedev&utm_medium=yt-annt)
- [Gulp 4.0 어떤 것들이 달라졌을까?](http://programmingsummaries.tistory.com/393)
