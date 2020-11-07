# Gradle

## Lifecycle of Gradle

- **Initialization phase**: determine which all project will take part in the build. This is done by the `include` statements in `settings.gradle`
- **Configuration phase**: executes the code in the build files. It creates everything required to run task. First gradle configure the root project and then the child projects.
- **Execution**: Execute the tasks.

To see this lifecycle we have to put some `println` statements:

- **For initialization phase**: in `settings.gradle`
- **For configuration phase**: in `build.gradle` in sub project as well as in parent `build.gradle`, under `allprojects` section.
- **For execution phase**: nothing as we have already created some dummy task called `hello`.

run `./gradlew hello`

## No need to create separate build.gradle

tell gradle that this is multi module project by including the below line.

`include 'sub-project-1', 'sub-project-2'`

then type `./gradlew projects`

> sub-project-* folders aren't required

## To pass configuration to all the project

> USE THIS: common functionality for all projects
in build.gradle

```groovy
allprojects {
    task('hello').doLast {
        println "I'm in $project.name"
    }
}
```

execute the task by name: `./gradlew hello`

> sub-project-* folders aren't required

## another way, create sub-project folders

> USE THIS: when we need to create customise behaviour
- create a folder with the same name as in settings.gradle
- create a file build.gradle
- create a task

```groovy
task('goodbye').doLast {
    println "goodbye from $project.name"
}
```

## specify the order of the task execution

By default, the tasks are executed `breadth-first` order. But we can change the order.

for that we have create `task-1` and `task-2` in the sub project.

mention the order in parent `build.gradle`

```groovy
evaluationDependsOnChildren()
Project subProject1 = project('sub-project-1')
Project subProject2 = project('sub-project-2')

subProject1.tasks['task-1'].mustRunAfter(subProject2.tasks['task-2'])
```

## Common commands

```shell script
./gradlew tasks --all
```