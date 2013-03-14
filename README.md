## What it does

Simply add this script to your client side assests after Backbone has been loaded.

This script hooks into the `Backbone.Router.route` method and checks the return values of your filters before triggering the actual event that calls to your router's callback for a particular route.

A before filter is expected to return `true` or `false` and can contain additional logic to redirect by using the Backbone.Router.navigate function.

## Usage

Create a method in your `Backbone.Router` called `beforeAllFilters` that returns an array of filter functions, the order of the filters in the array is the order in which they will be executed:
```javascript
this.beforeAllFilters = function() {
  [this.sessionFilter, this.authenticationFilter]
}

this.authenticationFilter = function (route, callback) {
    if (app.session.authenticated())
      return true;
    else {
      this.navigate('login', {trigger: false, replace: true});
      this.login();
      return false;
    }
}

this.sessionFilter = function() {
  if (!this.session) {
    this.session = new session(this.currentUser)
  }
  return true;
}
```