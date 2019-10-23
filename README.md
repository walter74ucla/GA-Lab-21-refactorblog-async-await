![GA Logo](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Lab: Handling [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) with [`async`/`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

1.  Make a branch in your blog app called `async-dev`.

2.  Complete the edit and update routes using `async`/`await`. Here is a task and some user stories.
  * **Task:** "Change Author when Editing an Article"
  * **User Stories**
    * User should be able to click "Edit" and see an edit page for an article with the information already filled in
    * Edit page should also show a drop-down menu containing all the Authors, with the current Author selected
    * When user submits their edits after selecting a new Author, the database should be updated to update the Article information and have the Article belong to the newly selected Author.
    * The user then should be taken to a page where they can see that their edits were succesfully made in the DB.
    * > hint: If you need help with the logic, you can refer to yesterday's [lesson plan](https://git.generalassemb.ly/WebDev-Connected-Classroom/two-model-relationship-build/blob/master/README.md)

3.  Refactor the rest of your routes to use `async` and `await`.

4.  Merge your branch back into your master branch after it is working.

--- 

## Hungry for More?

#### 5.  Run several asynchronous actions at the same time.

Try to look up how to use `Promise.all()` with `await` to run several asynchronous actions concurrently. This is a nice technique if you need to do several asynchronous actions that don't depend on each other, and you don't want/need to wait for one to finish before you run the next.  [This stack overflow post](https://stackoverflow.com/questions/35612428/call-async-await-functions-in-parallel) may help.

---

#### 6. Understand the structure of an Express app at a deeper level.  And use `next`.

[Read this](https://expressjs.com/en/guide/writing-middleware.html).  And then [read this](https://expressjs.com/en/guide/using-middleware.html).  

Ponder this: "An Express application is essentially a series of middleware function calls."  Whoa.

Each function can access the `request` (`req`) and `response` (`res`) objects, and typically either responds to the client or proceeds to the `next` function call in the series.  So all of the route handlers we've written (which also follow this pattern), could also be written to take an extra parameter `next`.  This callback refers to the next function that should be executed in the series of function calls.  

One nice basic usage of `next` is, in your `try`-`catch` blocks, if you catch an error that is thrown, instead of just `res.send()`ing the error to directly the user, you can pass it to the default error handling code that is built into express using `next()`. This is a sligtly more elegant way to handle errors because express's error handling code might be able to add some more useful information to the error messages than what you might otherwise see. See below for an example.

Example:

```
// donut index route
app.get('/donuts', async (req, res, next) => {
  try {
    const arrayOfDonuts = await Donut.find({});
    res.render('donuts/index.ejs', { donuts: arrayOfDonuts });
  }
  catch (error) {
    next(error); // PASS THE ERROR TO THE ERROR HANDLING FUNCTIONS BUILT INTO EXPRESS.
                 // (i.e. let express handle the error for you.)
  }
}
```

**Task**: Edit your routes to use `next` for error handling, similar to the above example.

---

#### 7. **Write your own error handling code**.  

Later, you could even come back and write your own error handling code that perhaps shows the user a nicer page, as most users probably shouldn't be seeing the stack traces and mongoose errors that you find so useful.  [Read this for more information about how](https://expressjs.com/en/guide/error-handling.html).
