This is the [Jekyll](http://jekyllrb.com/) source code for the [Project Project](http://people.reed.edu/~ormsbyk/projectproject/) website.

## Installing Jekyll on macOS

Installing Jekyll on your computer enables you to build the website and preview changes to it locally. To install Jekyll on macOS:

1. Install Xcode command line tools by running `xcode-select --install` and following the popup dialog.

2. Install [Homebrew](https://brew.sh/):

   ```
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```

3. Add `/usr/local/bin` to your path:

   ```
   echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
   ```

4. Install a newer version of Ruby:

   ```
   brew install ruby
   ```

5. Finally, install Jekyll:

   ```
   gem install jekyll
   ```

## Writing a new post

1. Create a text file in the `_posts` directory. The filename should be of the format
   
   ```
   <year>-<month>-<day>-<dasherized post name>.md
   ```

   So for example, a post published on 5 January 2017 with the title "Hopf Fibrations" would have the filename

   ```
   2017-01-05-hopf-fibrations.md
   ```

2. In the beginning of the text file, add the metadata for the post between triple dashes. For example, here's the metadata for [Cameron's post](http://people.reed.edu/~ormsbyk/projectproject/posts/greens-theorem.html):

   ```
   ---
   layout: post
   title: Green’s Theorem
   author: Cameron Fish
   category: Multivariable Calculus
   feature:
     file: greensdemofixed.stl
     file_type: stl
   resources:
     - name: 3D Model
       file: greensdemofixed.stl
   ---
   ```

3. After the triple dashes, add the post content with Markdown formatting. A guide on authoring Markdown for Project Project is available [here](http://people.reed.edu/~ormsbyk/projectproject/AUTHORING.html). A complete example of a Markdown authored text file for a post is available [here](https://raw.githubusercontent.com/chnn/project-project-website/master/_posts/2017-07-14-greens-theorem.md).

   If you have any images, STL files, or other assets that belong with your post, place them in the directory `assets/posts/<dasherized post name>/`. So for our "Hopf Fibrations" post, images would be placed in the `assets/posts/hopf-fibrations/` folder.

## Previewing posts

1. Change into the website directory with `cd`.
   
2. Run the Jekyll development server:

   ```
   jekyll serve
   ```

3. Preview the site in your web browser at http://localhost:4000/~ormsbyk/projectproject/. The site will be rebuilt as you update files. Refresh the web browser to see the changes.

## Deploying the site

1. If the `jekyll serve` development server is still running, stop it by pressing `Control-C`.

2. From the root of the website directory on your local machine, run the deploy script:

   ```
   ./deploy
   ```

## Miscellaneous notes

* If you want to deploy the site but also have draft posts that aren't ready to be published yet, you can add `published: false` to the draft posts' YAML frontmatter or place them in the `_drafts/` folder.
* Full documentation for everything that Jekyll is capable of is at http://jekyllrb.com/.
* Questions? Contact @chnn.

