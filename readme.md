#SalesHub - Development Styleguide

##HUBSPOT LOCAL SERVER

###Local File Structure for Gulp

        |--local-hubl-server
           |--bin
           |--conf
           |--docs
           |--lib
           |--work
               |--hubthemes
                 |--vast
                    |--custom
                       |--blog
                       |--dev
                          |--base
                          | *Include base HubSpot, Foundation, etc SCSS partials*
                          |--global
                          | *Include global SCSS partials, such as "__header.scss", "__mediaQueries.scss"*
                          |--pages
                          | *Include partials for each page*
                             |--[main.scss]
                             | *This includes main variables and styles*
                             |--[main.js]
                             | *This is the working JS file. Note that $(window).load() may need to replace $(document).ready() during development in order for functions to work. This should be changed back in the final file.*
                       |--email
                       |--page
                          |--landing-page-basic
                          | *Include HTML files for landing page templates*
                          |--web-page-basic
                          | *Include HTML files for web page templates*
                       |--styles
                          |--default
                             |--[hs__default__custom__style.css]
                             |--[main.js]
                             | *These are the final CSS/JS files to upload*
                       |--system
                          |--error-pages
                          |--global
                          | *This includes global modules such as "header.html"*
                          |--password-pages
                          |--subscription-preferences

###FTP Transfer

1. Connect to FTP:
  - __Host__: ftp.hubapi.com
  - __Port__: 3200
  - __Protocol__: FTP
  - __Encryption__: Require explicit FTP over TLS
  - __Logon Type__: Normal
  - __User__: [hubspot user id]
  - __Password__: [hubspot password]

2. Navigate to the correct __portal__ > __content__ > __templates__ > __custom__

3. Global modules will need to be uploaded first.
  - From the __custom__ folder, navigate to __system__ > __global__
  - Transfer all files in the __global__ folder from the local site to the remote site

4. Page transfers
  - Transfer all HTML files from the __blog__, __email__, __page__ folders to the corresponding folders in the remote site

5. CSS and JS transfers
  - The final/compiled CSS and JS files are located in __custom__ > __styles__ > __default__
  - Transfer these files to the corresponding folder in the remote site

6. The transferred files will be located in Coded Files in the HubSpot Design Manager
  - The CSS file is already named to be the primary stylesheet, so it should automatically link
  - The JS file will need to be linked in the footer