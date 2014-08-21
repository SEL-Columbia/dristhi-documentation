dristhi-documentation
=====================

#Documentation for Project Dristhi


## How to run documentation site in dev machine?


### Prerequisites

* Make sure the machine has the following gems installed:
  
  - `jekyll`
  
  - `bundler`

* Check if they are installed:

    `gem list | grep jekyll`

* If not, install it using:

    `gem install jekyll`


* Checkout this repository and change your working directory to the repo

* Now install all the required gems using:

    `bundle install`

### Running the site locally

* Run:

    `bundle exec rake generate`
    
    `bundle exec rake preview`
    
* The site will be running under port `4000` and you can access [here](http://localhost:4000/dristhi-documentation)


### Deploying the changes to prod

* Do all the necessary changes needed and make a commit locally. We can either push to master before/after the deploy. 

* If you are deploying for the first, do the following steps

    1. Run the following rake task
        
        `bundle exec rake setup_github_pages`
        
        This task will create directory called `_deploy` and initializes the git repository and start tracking the remote branch `gh-pages`. But we will have to manually add this information too.
        
    2. Change the working directory to `_deploy`. Open the `.git/config` and add:
    
            [branch "gh-pages"]
                remote = origin
                merge = refs/heads/gh-pages
            [branch "master"]
                remote = origin
                merge = refs/heads/master
    

* Deploy the site using `bundle exec rake deploy`.


        
        


