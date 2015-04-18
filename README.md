
# earl-gulp

Earl Grey macros for gulp

To use, write your tasks in `gulpfile.eg`, and then create a
`gulpfile.js` file that requires `earlgrey/register` (`npm install
earlgrey --save-dev`) and `./gulpfile.eg`

To define a task:


    require-macros:
       earl-gulp -> task

    ;; No dependencies
    task some-task:
       task-code

    ;; Task with one dependency
    task other-task < some-task:
       task-code

    ;; Task with multiple dependencies
    task greatest < {some-task, other-task}:
       task-code

    ;; Tasks can have no body
    task default < greatest


For example:


### gulpfile.js

    require("earlgrey/register");
    require("./gulpfile.eg");


### gulpfile.eg

    require-macros:
       earl-gulp -> task

    require:
       gulp, gulp-sass, gulp-earl, gulo-sourcemaps

    task sass:
       chain gulp:
          @src{"./content/**/*.sass"}
          @pipe{gulp-sass{indented-syntax = true}}
          @pipe{gulp.dest{"./output"}}

    task earl:
       chain gulp:
          @src{"./content/**/*.eg"}
          @pipe{gulp-sourcemaps.init{}}
          @pipe{gulp-earl{}}
          @pipe{gulp-sourcemaps.write{"./"}}
          @pipe{gulp.dest{"./output"}}

    task default < {earl, sass}


