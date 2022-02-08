MUSA 801: Good Code Habits
================
Matthew Harris
2/7/2022

## Musa 801 - Code Habits

The purpose of this markdown is to document some of the better code
habit that you will use in MUSA 801 - Practicum. This will be a
living/growing document as new ideas come up. Much of the code is here
will be a refresher from previous MUSA classes. If you find something
his here that does not work, please let me know!

### GitHub

Using Github is important for 1) you to learn an industry standard
platform that will help you code collaboratively, 2) your team to stay
up to date and work more efficiently, 3) for the instructors to observe
your work progress and have a single source of code in the future.

Rstudio has a built in GitHub panel that helps you do the most common
actions like `Pull`, `Push`, and `Commit`, as well as create and use
branches. The GitHub desktop app is also a good tool, but the Rstudio
integration can be more efficient.  
### Rstudio Projects & Code Managment

#### Projects

First rule, use Rstudio `Projects` for every thing you do. Projects
allow you to keep your code and artifacts all in one folder and keep the
‘environment’ separate from other projects. The environment is all of
the code, objects, functions, libraries that are stored in your
computers memory at a given time for a given project. Secondly, you need
to use projects in order to use GitHub as described above.

#### RData

Also, the setting for `Restore .RData into workspace at startup` should
NOT be checked. You do not want to load the previous work space when you
open your project. Additionally, make sure that the
`Save workspace to .RData on exist` is set to `never`. With these
settings, Rstudio will never save your environment when you exit (or it
crashes). That means if you fit a bit model and you close the project,
that model will NOT be there when you open it again. That might seems
like a bad thing, but knowing that your R objects are only temporary
makes you use better practices for file management and redundancy. For
example, you will need to save objects as either csv, geojson, RDS, or
other formats along the way in order to save time re-computing them each
time you run your code.

#### File management

Try to always keep the files relevant to your project within your
project folder. Avoid calling files (e.g. CSV or SHP) from your user
folder or a temp folder somewhere on your hard drive. If your code uses
a file, that file should be in your project folder. If there are files
that are too large or sensitive and should not be in GitHub, use
`.gitignore` to ignore the folders that contain those files.

#### Sourcing files

R scripts can call other R scripts. This is helpful is you have
functions or data cleaning steps that you use often, but do not want to
cut/paste and repeat in all of your scripts. Having a single source file
of your functions both save lines of code in your scripts, but also
reduces the risk of cut/paste errors or using the wrong version of a
function. To call a script into your script, simply use
`source(my_functions.r)`.

### Tidy code with `tidyverse`

You should be using the `tidyverse` for the vast majority of your data
cleaning, prep, and model building code. using `library(tidyverse)`
should be sufficient to load the relevant packages. Try to use packages
like `sf` that play well with the tidy way of working.

### Tidy models with `tidymodels`

### Markdown maintinance

Creating a nice looking markdown will help you communicate the awesome
data science you are doing. A nice looking markdown is one that has a
table of contents, graphics that large and readable, folded code chunks,
and limited (if any) out put from the console. That last point includes
purposeful output including tables and regression coefficients, as well
as unintended output such as package warnings or loop interactions. Use
combinations of `echo = FALSE` and … in the top of each code chunk to
control these behaviors.

### API Calling

Increasingly, the data you need is accessible through an API
(Application Programming Interface). The most common API type is calls a
`REST` API and is accessed in R by writing code for `GET` and sometimes
`POST` queries. The `httr` package is very helpful here. Also, most of
the time the data from an API will be returned as a `JSON` object and we
use the `jsonlite` package to help convert it to something useful.

There are other APIs that have more proprietary standards such as the
Socrata API. The `RSocrata` package helps with access to these APIs.

The most basic form of a REST API call is a `GET` call to the API
address

``` r
library(httr)
library(jsonlite)

res = httr::GET("http://api.open-notify.org/astros.json")

str(res)
```

    ## List of 10
    ##  $ url        : chr "http://api.open-notify.org/astros.json"
    ##  $ status_code: int 200
    ##  $ headers    :List of 6
    ##   ..$ server                     : chr "nginx/1.10.3"
    ##   ..$ date                       : chr "Tue, 08 Feb 2022 15:08:35 GMT"
    ##   ..$ content-type               : chr "application/json"
    ##   ..$ content-length             : chr "497"
    ##   ..$ connection                 : chr "keep-alive"
    ##   ..$ access-control-allow-origin: chr "*"
    ##   ..- attr(*, "class")= chr [1:2] "insensitive" "list"
    ##  $ all_headers:List of 1
    ##   ..$ :List of 3
    ##   .. ..$ status : int 200
    ##   .. ..$ version: chr "HTTP/1.1"
    ##   .. ..$ headers:List of 6
    ##   .. .. ..$ server                     : chr "nginx/1.10.3"
    ##   .. .. ..$ date                       : chr "Tue, 08 Feb 2022 15:08:35 GMT"
    ##   .. .. ..$ content-type               : chr "application/json"
    ##   .. .. ..$ content-length             : chr "497"
    ##   .. .. ..$ connection                 : chr "keep-alive"
    ##   .. .. ..$ access-control-allow-origin: chr "*"
    ##   .. .. ..- attr(*, "class")= chr [1:2] "insensitive" "list"
    ##  $ cookies    :'data.frame': 0 obs. of  7 variables:
    ##   ..$ domain    : logi(0) 
    ##   ..$ flag      : logi(0) 
    ##   ..$ path      : logi(0) 
    ##   ..$ secure    : logi(0) 
    ##   ..$ expiration: 'POSIXct' num(0) 
    ##   ..$ name      : logi(0) 
    ##   ..$ value     : logi(0) 
    ##  $ content    : raw [1:497] 7b 22 70 65 ...
    ##  $ date       : POSIXct[1:1], format: "2022-02-08 15:08:35"
    ##  $ times      : Named num [1:6] 0 0.0269 0.1027 0.1029 0.1825 ...
    ##   ..- attr(*, "names")= chr [1:6] "redirect" "namelookup" "connect" "pretransfer" ...
    ##  $ request    :List of 7
    ##   ..$ method    : chr "GET"
    ##   ..$ url       : chr "http://api.open-notify.org/astros.json"
    ##   ..$ headers   : Named chr "application/json, text/xml, application/xml, */*"
    ##   .. ..- attr(*, "names")= chr "Accept"
    ##   ..$ fields    : NULL
    ##   ..$ options   :List of 2
    ##   .. ..$ useragent: chr "libcurl/7.64.1 r-curl/4.3.2 httr/1.4.2"
    ##   .. ..$ httpget  : logi TRUE
    ##   ..$ auth_token: NULL
    ##   ..$ output    : list()
    ##   .. ..- attr(*, "class")= chr [1:2] "write_memory" "write_function"
    ##   ..- attr(*, "class")= chr "request"
    ##  $ handle     :Class 'curl_handle' <externalptr> 
    ##  - attr(*, "class")= chr "response"

The content of the response in raw JSON form

``` r
rawToChar(res$content)
```

    ## [1] "{\"people\": [{\"craft\": \"ISS\", \"name\": \"Mark Vande Hei\"}, {\"craft\": \"ISS\", \"name\": \"Pyotr Dubrov\"}, {\"craft\": \"ISS\", \"name\": \"Anton Shkaplerov\"}, {\"craft\": \"Shenzhou 13\", \"name\": \"Zhai Zhigang\"}, {\"craft\": \"Shenzhou 13\", \"name\": \"Wang Yaping\"}, {\"craft\": \"Shenzhou 13\", \"name\": \"Ye Guangfu\"}, {\"craft\": \"ISS\", \"name\": \"Raja Chari\"}, {\"craft\": \"ISS\", \"name\": \"Tom Marshburn\"}, {\"craft\": \"ISS\", \"name\": \"Kayla Barron\"}, {\"craft\": \"ISS\", \"name\": \"Matthias Maurer\"}], \"message\": \"success\", \"number\": 10}"

Converting the content from JSON to a data frame or character vectors

``` r
data = fromJSON(rawToChar(res$content))
data$people
```

    ##          craft             name
    ## 1          ISS   Mark Vande Hei
    ## 2          ISS     Pyotr Dubrov
    ## 3          ISS Anton Shkaplerov
    ## 4  Shenzhou 13     Zhai Zhigang
    ## 5  Shenzhou 13      Wang Yaping
    ## 6  Shenzhou 13       Ye Guangfu
    ## 7          ISS       Raja Chari
    ## 8          ISS    Tom Marshburn
    ## 9          ISS     Kayla Barron
    ## 10         ISS  Matthias Maurer

Quite often, an API will take parameters to define the query. It would
not be efficient to download a full data set over an API call each time
you want a few rows. For this reason, APIs take queries parameters to
filter for only the data you want. in the example below, the API takes
the query parameters of `lat` and `lon` to get a result specific to a
geographic location.

``` r
res = GET('http://api.open-notify.org/iss-pass.json',
          query = list(lat = 40.7, lon = -74))
```

### API Building via `plumber`
