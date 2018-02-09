# vue-pomodoro

This Vue project is a simple pomodoro timer with two separate interval clocks. The intended use is for doing cardio exercise
where you wanting one overall timer for the entire exercise time, but you also want two shorter intervals of different lengths
so that you can have 'sprint' intervals and 'rest' intervals.

## To Use

Use the overall timer + or - buttons to add time to the overall exercise timer. Use the interval timer + or - buttons to adjust
the length of your intervals. Use the 'Start', 'Pause', and 'Reset' buttons to start, pause, or reset the timers.

## Contributing

This project is still in active development. Development is based on the development branch. To contribute, follow these steps:

* `git clone https://github.com/matty22/vue-pomodoro.git`
* `git checkout development`
* `git checkout -b issue-label/reference-to-issue` (ex. bug/fix-grammar-error)

Make your changes in your local copy...

* `git add [filename].ext`
* `git commit -m "Concise message that explains your changes"`
* `git checkout development`
* `git pull`
* `git checkout fix/my-branch`
* `git merge development`
* `git push origin fix/my-branch`

Then, open a PR on the development branch on the repo.

For questions, read these three blog posts for guidelines on how to contribute:
* "How to not f- up your local files with Git [Part 1](https://medium.com/@francesco.agnoletto/how-to-not-f-up-your-local-files-with-git-part-1-e0756c88fd3c), [Part 2](https://medium.com/@francesco.agnoletto/how-to-not-f-up-your-local-files-with-git-part-2-fc4e243be02a), and [Part 3](https://medium.com/chingu/how-to-not-f-up-your-local-files-with-git-part-3-bf03b27b6e64)"

## Vue-CLI Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
