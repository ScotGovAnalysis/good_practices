# Good Coding Practices
Writing good quality code makes life easier for other people and your future self. It can also reduce the risk of errors (h/t to [Best Coding Practices for R](https://bookdown.org/content/d1e53ac9-28ce-472f-bc2c-f499f18264a3/#coverpage) by Vikram Singh Rawat for much of this content).

## :heavy_check_mark: Create a Project
### Do
Work within an [R Project](https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects).

* R Projects act as your working directory, where the root is the directory created, or chosen, when setting up a new project.  
* Once you create a project it is easier to manage your files and folders and it’s easier to give it somebody as well.
* With bigger projects, consider creating sub-folders for e.g. code (.R, .Rmd), data (.csv, .data, .rds) and outputs (.docx, .png, .pdf).

## :heavy_check_mark: Code structure
Once you have arranged the files and folders in a logical way, then comes the fact that the code itself should be arranged in such a way that feels easy to parse. Always remember: **Code is read more often then it’s written**.

### Commenting code
> "When you feel the need to write a comment, first try to refactor the code so that any comment becomes superfluous." – Martin Fowler, Refactoring

#### Do
* Explain _**why**_ it is doing what it is doing. This will make it easier for users to know what the code is for

    #### Good Comment:
    ``` 
    data %>% 
       select(1:161)  # column 161 is where the data stops and it becomes footnotes
    ```
    #### Bad comment:
    ``` 
    data %>% 
       select(1:161)  # selecting columns
    ```

#### Don't
* Just explain *what* it is doing
* Keep any commented *just-in-case* code that isn't needed
* Include #TODO notes within the the code. Keep these separate in a README or in the GitHub issues tab
* Rely on (un)commenting code to change behaviour
    * You can quickly lose track!

### Create sections
RStudio gives you an ability to create section with `ctrl` + `shift` + `r`, or you can create one by adding 4 dashes (-) or 4 hash symbol (#) after a comment. The same applies for RMarkdown. For example:

    ```
    # Load data ----

    # Data wrangling ####
    ```
It also helps you jump between sections using `shift`+ `alt` + `j`. You can easily switch between sections and fold them at will. It helps you not only in navigation but maintaining a layout of the entire code as well.

You can also create sub-section in R by adding hash symbols in front of a section:

    ```
    ## some comment ----

    ### another comment ----

    #### and yet another comment ----
    ```
### Order your code
When you write code there are standard practices that are used across domains that you should definitely use, in the following order:

1. Call your libraries.
2. Set all default variables or global options and all the path variables.
3. Source all the code.
4. Call all data files.

This coherence keeps all your code easy to find. 

### Indendation
Indentation makes code readable. No matter what language you work in, your code should be properly indented so that the nature of the code is understandable.

   #### Bad:

    ```
    foo <- function(first_arg, second_arg, third_arg){
    create_file <- readxl::read_excel(path = first_arg, sheet = second_arg, range = third_arg)
    }
    ```

   #### Good:

    ```
    foo <- function(

      first_arg, second_arg, third_arg

    ){

    create_file <- readxl::read_excel(path = first_arg, 
				          sheet = second_arg, 
                                      range = third_arg)
    }
    ```

### Give your code breathing room

   #### Bad:

    ```
    y=ts(data=c(23,391,728,512,10),start=2010)
    ```

   #### Good:

    ```
    y = ts(data = c(23, 391, 728, 512, 10), 
           start = 2010)
    ```

In RStudio you can press `ctrl` + `shift` + `a` to autoformat your code.

## :heavy_check_mark: [Write code for humans](http://douglasorr.github.io/2020-03-data-for-machines/article.html)
> "Code is read more often than it is written." — [Guido van Rossum](https://twitter.com/gvanrossum) (creator of Python)

> "Programs are meant to be read by humans and only incidentally for computers to execute."  
> — Donald Knuth, The Art of Computer Programming

Writing code that humans can understand is an investment to have maintainable and reusable code. If it is easy to read, maintenance and  edits will be much quicker in the future and it will be much easier for other people to work with your code. 
The shortest, most efficient code for the computer is likely not the optimal code for human readability. 

### Naming conventions
Proper naming conventions will help collaboration in big teams and it makes the code easier to maintain. The three most widely-used conventions amongst programming communities are:
* camelCase: Names start with a small letter and every subsequent will start with upperCase
* PascalCase: Similar to camelCase but the first letter of the string is also UpperCase
* snake_case: All names are lower case with an underscore between words

Whichever convention you choose, make sure to stick with it for the duration of your project.

### Maintain meaningful and proper object names
  
`height_in_metres()` is better than `converter()` for a function that converts height into metres. This makes it more obvious what your code is doing and makes your code more readable

### Maintain meaningful and proper column names
Most of the data we read is from either an Excel file or a poorly designed database, thus we see column names with spaces and dots and other rogue characters. Remember these rules for column names:

1. Stick to consistent naming convention (see above)
2. Assume everything is case-sensitive.
3. Do not use special characters ever.
4. Do not add spaces.

### Do not use numbers to specify columns
In R you can use numbers to refer to columns, but (to parapharse a noted scholar) just because you can doesn’t mean you should.

   #### Bad:

    ```
    # In base R
    mtcars[,c(1,3:5,8)]

    # Using tidyverse
    mtcars %>% select(1, 3:5, 8)
    ```

   #### Good:

    ```
    # In base R
    mtcars[, c("mpg", "disp", "hp", "drat", "vs")]

    # Using tidyverse
    mtcars %>% 
      select(mpg, disp, hp, drat, vs)
    ```

### Conditional statements

#### Do
* Use `case_when()` within the `tidyverse` collection for conditional statements. This is a more readable way of dealing with many ifelse statements. Standard `if_else` statements can be very useful but can become hard to understand if too many are used.
#### Don't
* Use deeply nested `for` loops or `if_else` statements

 
## :heavy_check_mark: Use a style guide 
The default for Scottish Government is the [Tidyverse styleguide](https://style.tidyverse.org/)  

Lint your code:  
* **Linting**: The automated checking of your source code for programmatic and stylistic errors
* [Lintr](https://github.com/jimhester/lintr) checks your code for style, syntax errors and possible semantic issues  

## :heavy_check_mark: Avoid absolute file paths
#### Do
* Use relative file paths
    * Use the library [`here()`](https://www.rdocumentation.org/packages/here/versions/1.0.0)
      * `here()` finds your project files based on the current working directory at the time when the package is loaded.
#### Don't
* Use absolute file paths as this reduces the reproducability of the code
    * It won't work on another users computer
    * It won't work if any files move
      
## :heavy_check_mark: Keep scripts short
#### Do
* Ideally keep to fewer than 250 lines. This makes scripts more manageable
* Break up and `source()` sections like data processing, and variable or function assigning
#### Don't
* Keep everything for a large project in one script  
    * For example, break up the UI, Server & Golbal sections of a large [Shiny app](https://shiny.rstudio.com/articles/basics.html)

## :heavy_check_mark: Use functions
> "You should consider writing a function whenever you've copied and pasted a block of code more than twice." – H. Wickham (our lord and saviour).

Functions allow you to automate common tasks rather than repeatedly writing the same code. [R for Data Science](https://r4ds.had.co.nz/functions.html) provides a good explanation for using functions. 

(tl;dr: **D**on't **R**epeat **Y**ourself [(DRY)](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself))

#### Do
* Use several smaller functions rather that one large one. This includes small, well named, helper functions  
* Use the built in proper functions to simplify  
* `if(is.numeric())` is better than `if(class(x) == "numeric" || class(x) == "integer")` 
* Use double colon when calling functions if you have not loaded a library already
    * E.g. `readxl::read_xlsx()`
    * This makes it more obvious where the function is coming from
    * And avoids ambiguous code when two packages have a function with the same name

#### Do not
* Repeat yourself
 
## :heavy_check_mark: Automate checks for input data
Automate the new data checks so you don't miss anything unexpected. Especially if you expect the data to be updated.

For example, automate checks for:
* Outliers
* NA's
* Class
* Expected column names
      
## :heavy_check_mark: Code in the open 
#### Do
* Coding in the open encourages:
     * The use of good code practice so others can read your code
     * Collaboration
     * Code reviews and [pull requests](https://docs.github.com/en/free-pro-team@latest/desktop/contributing-and-collaborating-using-github-desktop/creating-an-issue-or-pull-request)
* Code in the open if you can, e.g. use [Scottish Government Analysis](https://github.com/ScotGovAnalysis)
#### Don't
* Share sensitive information such as unpublished data, API keys or passwords

## :heavy_check_mark: Make a README 
Here is the [Gov Data Science guidance on writing READMEs](https://gds-way.cloudapps.digital/manuals/readme-guidance.html#writing-readmes)  
#### Include:
* Contact details
* How to run the code
* A licence (Scottish Government default is [Open Government Licence](http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/))
* An estimate of run time if it takes longer than a couple minutes
* README's should help the user: 
    * Understand what the project is
    * Learn how to use the project

## :heavy_check_mark: Include library version number
Make a note of the library version number incase future updates to the library break the functionality of your code. In doing this, anyone looking at the code in the future can see when it last worked and begin troubleshooting. 

## Licence
All content is available under the [Open Government Licence v3.0](http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), except where otherwise stated.
