---
layout: post
title:      "React Redux Project"
date:       2019-05-13 05:52:29 +0000
permalink:  react_redux_project
---


The final project, 5 months and it comes down to this one!  I have to say, it was a challenge, a very welcome challenge though.   Initially I started working on a user login, but after doing some research and asking our Cohort lead about the process, it can be a bit challenging, hopefully that will be a feature that I will add later!  

I started with `npx create-react-app`, the repo for this can be found [here](https://github.com/facebook/create-react-app).  The repo has a great amount of info about the create-react-app snippet and what it does.  You start off with a fully operational single page web app!  After removing a few default items, I started building out some of the view components as well as the navigation for those.  React has an awesome import called NavLink which makes it appear to the user they are navigating to different pages.
```
import { NavLink } from 'react-router-dom'
..
<NavLink className='buttonNav' to='/' exact>Home </NavLink>
<NavLink className='buttonNav' to='/favorites' exact> Favorites </NavLink>
<NavLink className='buttonNav' to='/about' exact> About </NavLink>
```
When a user navigates to home, they se the base URL, favorites will show the `https://baseURL/Favorites` and About will do the same thing, all while remaining on a single HTML.  I little bit of code is then required to display it where you wish but it's pretty straightforward.  The about component doesn't take state, props, its only job is to show information about the web app. Favorites, you could probably guess, will be showing the users favorites!  The home page is where most of the action happens, it contains the search bar and will also render out the results from that query.

The search bar is a form with a single field and when the user types in their query it then saves that to state and holds that there until the submit button is pressed.  The code below updates the field while also saving the query to the state.
```
  handleChange = (e) => {
    this.setState({
      [e.target.name]: e.target.value
    })
  }
```
There is another function that handles submit, it stops the default re-render of the page and then calls an action and sends the query along with that action.  My first fetch request is done with axios, it adds the query to the end of the API url and then dispatches an action which attempts to retrieve the data from the api.  Thunk middleware allows us to return functions instead of actions as well as allowing asynchronous dispatch.

Depending on the status, the reducer will either show that it is still loading or once loaded, it will send the retrieved data to the state within the component with `mapStateToProps`.  
```
function mapStateToProps(state) {
  return {
    beers: state.beersReducer.beers,
    loading: state.beersReducer.loading,
  }
```
Once there, we can access that data in that components props.  Each item shown has a Save Favorite button which saves the selected item to an external rails API, its only job is to provide a sort of database which we can use to manipulate the data.

```
export const addFavBeer = (data) => {
  return dispatch => {
    dispatch({type: "ADD_FAV_BEERS"});
    return fetch('/beers', {
      method: 'POST',
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        name: data.name,
        description: da...
      })
    })
  }
}
```
Here we are exporting a constant called addFavBeer which POSTs to the external API and turns the data into JSON.  The actions to post, delete, or fetch the data are all pretty similar and give us access to the rails API.  Theres a bit more logic in here to verifying data, check for errors among other things but this is the quick rundown of my React project.  My repo can be found [here](https://github.com/NInman112/what-ales-you)!
