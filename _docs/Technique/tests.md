---
title: Decidim and tests
category: Technique
order: 1
---

# Decidim and tests, a loving story.

Decidim use tests to ensure functionnality and prevent regression.

1. How to run tests suite
2. What should I test?
3. I want to run tests like circle-ci.

## 1. How to run tests suite

N.B.: Check the tests suite with `rake -t | grep test`

* ### I want to test all decidim component.
  *Easy!* To test all decidim component  use 
  ```
  bundle exec rake test_all
  ```
  
* ### I want to test only a component
  *Easy!* In order to test all component, you need to run a task which initiate the test database, included in test_all. 
  You need to run this task only in the first time.
  ```
  bundle exec rake test_app
  ```
  then:
  ```
  bundle exec rake test_<name of the component>
  ```
  
* ### I want to test only one file inside a component
  *Easy as always!* In order to only run a single test, you need to be inside the component directory : 
  `cd <component-directory>`
  Then : 
  ```
  bundle exec rspec ./spec/<path of the test>.rb
  ```

## 2. What should I test?
That a really good question, and for more information you can ask @mrcasals and @deivid-rodriguez on gitter. 
We think that testing everything is not necessary, but we think all behaviour must be tested.

What does that mean? That means you don't need to test if your controller is responding with a 200. But you need to test if the controller gives you the good value.
Ask yourself, what the user want to do? What the feature is exactly? And check if those behaviour can be achieve.

Even if you test everything with system tests, you must be sure of all business logic. 
Exemple, if you transform a user input, you should check if the correct informations are displayed, and if this specific behaviour is covered with unit test.

## 3. I want to run tests like circle-ci.
To check how circle-ci runs tests, you can test locally with :
```
 circleci local execute --job <name of the job>

```
The name of the job can be found in root path : `cat .circle-ci/config.yml`
  
