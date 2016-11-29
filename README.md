<!--
Creator: Team, edits by Cory Fauver
Market: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Angular`$http` Update and Delete

### Warmup

Without looking at any outside resources, write out the syntax for an Angular `$http` GET request on the board in pairs.

<details>
  <summary>
  </summary>
  ```javascript
  $http({
    method: 'GET',
    url: 'http://www.jonsnow-portfolio.com/api/projects'
  }).then(successCallback, errorCallback);

  function successCallback(response) {
    console.log('response for all projects:', response);
  }
  function errorCallback(error) {
    console.log('There was an error getting the data', error);
  }
  ```
</details>

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
![CRUD](http://2.bp.blogspot.com/-frXMwdxsUyg/VpwZAFmBcJI/AAAAAAAABK4/G6sBPW0qhg8/s1600/CRUD.png)

*This workshop is important because:*

Angular is built to help develop full CRUD apps. Update and delete are the more complex half of CRUD that require reflection on the DOM after it has been loaded. Practice is important to help you feel comfortable doing this work on your own.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Make requests to update and delete data in the DB using `$http`.
- Successfully update and delete data in the view once we receive a success message from the DB.

### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- make update and delete requests using jQuery's `$.ajax()` function.
- using jQuery to adjust the DOM to reflect data returned from the server.


### Update a database using `$http`

<details>
  <summary>
  **Pseudocode an update function on the client side**
  </summary>
  1. Gather the proper resources to send the request
    1. Specify the proper endpoint on the API to update the proper resource.
    2. Select the proper data to send in this PUT request.
  3. Make the PUT request with all of the proper data.
  4. If a successful response comes back, update the data in your view.
  5. If an error response comes back, log the error, optionally, notify the user of the error, and do not update the data in the view.
</details>
<br>
<details>
  <summary> **Update a resource -- with an example `$http` request to `PUT /api/books/:id`.** </summary>


  ```js
  vm.sendUpdate = function(book){
    $http({
      method: 'PUT',
      url: '/api/books/'+book._id,
      data: {
        title: book.title,
        author: book.author,
        characters: book.characters
      },
    }).then(function successCallback(response) {
      // update the data that's bound to the view.
    }, function errorCallback(error) {
      console.log('There was an error', error);
    })
  };
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": {
      _id: "56fd8372m098ok2u89uclwm09",
      title: "Harry Potter and the Sorcerer's Stone",
      author: "J.K. Rowling",
      characters: [ "Harry Potter", "Ron Weasley", "Hermione Granger", "Hagrid", "Dumbledore"]
    },
    "status": 200,
    "config": {
      "method": "PUT",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "data": {
        _id: "56fd8372m098ok2u89uclwm09",
        title: "Harry Potter and the Sorcerer's Stone",
        author: "J.K. Rowling",
        characters: [ "Harry Potter", "Ron Weasley", "Hermione Granger", "Hagrid", "Dumbledore"]
      },
      "url": "http://www.cf-books.com/api/books/56fd8372m098ok2u89uclwm09",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>


### Delete an entry in the database using `$http`

<details>
  <summary>
  **Pseudocode a delete function on the client side**
  </summary>
  1. Gather the proper resources to send the request
    1. Get the proper endpoint on the API to update the proper resource. Make sure you know the way you are supposed to identify a specific item to delete (by id? by name?). For example, `/api/albums/:id`.
  3. Make the DELETE request to the proper endpoint.
  4. If a successful response comes back, update the data in your view.
  5. If an error response comes back, log the error, optionally, notify the user of the error, and do not update the data in the view.
</details>
<br>
<details>
  <summary>**Delete a resource -- with an example `$http` request to `DELETE /api/books/:id`.**</summary>
  ```js
  vm.deleteBook = function(book){
    $http({
      method: 'DELETE',
      url: '/api/books/' + book._id,
    }).then(function successCallback(response) {
      // delete the entry from the data that's bound to the view.
    }, function errorCallback(error) {
      console.log('There was an error', error);
      // Possibly, display to the user that you were unable to delete.
    });
  };
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": {
      _id: "56fd8372m098ok2u89uclwm09",
      title: "Harry Potter and the Sorcerer's Stone",
      author: "J.K. Rowling",
      characters: [ "Harry Potter", "Ron Weasley", "Hermione Granger", "Hagrid", "Dumbledore"]
    },
    "status": 200,
    "config": {
      "method": "DELETE",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "url": "http://www.cf-books.com/api/books/56fd8372m098ok2u89uclwm0",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>

### The hard part
##### "If a successful response comes back, update the data in your view."

Why was this hard when we were using jQuery and Handlebars on the client side?

This step is quite a bit easier in Angular. We can simply change the data that's bound to the view.

<details>
  <summary>`.then` for delete.</summary>
```javascript
vm.deleteBook = function(book){
  $http({
    method: 'DELETE',
    url: '/api/books/' + book._id,
  }).then(function successCallback(deletedBookJson) {
    var index = vm.books.indexOf(book);
    vm.books.splice(index, 1);
  }, function errorCallback(response) {
    console.log('There was an error deleting the data', response);
  });
}
```
</details>

<details>
  <summary>`.then` for update.</summary>
```javascript
vm.updateBook = function(book){
  $http({
    method: 'PUT',
    url: '/api/books/' + book._id,
    data: {
      title: book.title,
      author: book.author,
      characters: book.characters
    },
  }).then(function successCallback(updatedBookJson) {
    var index = vm.books.indexOf(book);
    vm.books.splice(index, 1, updatedBookJson);
    // any hiding / showing that needs to occur
  }, function errorCallback(response) {
    console.log('There was an error deleting the data', response);
  });
}
```
</details>


### Independent Practice

[Sprint 3 of tunely-angular](https://github.com/sf-wdi-31/tunely-angular/blob/master/docs/sprint3.md) - Don't forget to checkout `solutions_sprint_2` and follow the [branching instructions](https://github.com/sf-wdi-31/tunely-angular/blob/master/docs/starting_with_a_branch.md#subsequent-sprints);

## Closing Thoughts
- Check in with yourself: Could you explain each piece of the `$http` service syntax? Could you write out the syntax without looking at a previous example?
- At this point in your practice, you should feel comfortable with the general idea of making calls from the client side to the server side, independent of the syntax or framework used. If you know the pieces required to make HTTP requests, you will quickly adopt new syntax from other frameworks you'll learn in the future.
- Looking forward: you will be using Angular to build a CRUD application this weekend.

## Additional Resources
- [`$http` documentation](https://docs.angularjs.org/api/ng/service/$http)
