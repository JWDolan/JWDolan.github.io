---
layout: post
title:      "What is React?"
date:       2017-12-07 20:56:10 -0500
permalink:  what_is_react_key_concepts
---


**DOM** = programmatic representation of the document in the browser
**Virtual DOM** = React’s copy of the HTML DOM, which can be slow
       --Builds a virtual representation of what the document should look like
       --When rendering views, the virtual DOM looks at the existing DOM, changes only what needs to be changed, and re-renders the changes
       --Good choice when building apps where changes in the interface happen often

**React** =a  library for building user interfaces by breaking them into components
       --Uses a full-featured language (instead of templates) to render views

**Declarative Programming**
Allows us to focus on what our application should look like
**Example**: a search component that filters a list of results
**Imperative**: manually update the DOM by removing the results that aren’t	relevant and adding the new ones to the list (“this is what you should do”)
**Declarative**: display the items that have been filtered (“it should look like this”)

**Setting up React**: npm install -g create-react-app webpack
**Running the Application**: npm install && npm start (from the directory of the project)

**React.createElement arguments**:
**1st**: element type (i.e. <h1> tag or another React component)
               --	for an HTML element, pass in the name as a string
               --	for another React component, pass in the variable the component is assigned to
**2nd**: object containing properties (‘props’) that get passed into the component
**3rd**: children of that component (i.e. quoted string as shown above (interpreted as text) or a reference to another component, allowing us to nest elements and components

**Rendering the element to the page using ReactDOM.render()**: takes two arguments
**1st**: what we want to render         
**2nd**: target DOM node to render things into

**If p and title are siblings with div as a parent**:
	import React from ‘react’;
	import ReactDOM from ‘react-dom’;

	const title = React.createElement(‘h1’, {}, ‘My First React Code’);
	const paragraph = React.createElement(‘p’, {}, ‘Writing some more HTML’);
	const container = React.createElement(‘div’, {}, [title, paragraph])

  ReactDOM.render(container, document.getElementById(‘global’));

**Adding attributes to children’s children, declared inline**:
	import React from ‘react’;
	import ReactDOM from ‘react-dom’;
	
	const list = React.createElement(‘div’, {}, 
		React.createElement(‘h1’, {}, ‘My favorite ice cream flavors’),
		React.createElement(‘ul’, {}, 
			[React.createElement(‘li’, {className: ‘brown’}, ‘Chocolate’),
		 	 React.createElement(‘li’, {className: ‘white’}, ‘Vanilla’),
			 React.createElement(‘li’, {className: ‘yellow’}, ‘Banana’)]));

	ReactDOM.render(list, document.getElementById(‘global’));
		
