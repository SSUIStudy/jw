
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






