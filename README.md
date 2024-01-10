# Redis Hugo site template

## Files and folders

* **/archetypes**: A Markdown file needs to have some front matter. An archetype defines which front matter is used when using `hugo new content`. Right now, the only supported archetype is the default one. **Note:** We might want to add additional archetypes in the future because most of our pages contain additional meta data properties like `linkTitle`. 
* **/content**: This folder contains the markdown files. We will have the subfolders `docs/develop`, `docs/integrate`, and `docs/operate``
* **/assets**: CSS files, site-wide icons and images
* **/data**: Data files that are accessed by Hugo. The data is then rendered with the help of short codes or partials.
* **/layouts/partials**: HTML templates that are used across sites. Examples are TOC-s, breadcrumps, or headers. 
* **/layouts/$type**: Each page type has at least the following templates to implement `single.html` and `list.html`. The `single` template is used to render a concrete page. The `list` template is used to render a collection (e.g., all sub-pages).
* **/layouts/home.html**: The home page of the site. So the page that is entered when you open the root path
* **/layouts/404.html**: The default 404 page.
* **/layouts/shortcodes** HTML template snippets that you want to leverage within Markdown files. Examples are tabbed code examples or notification blocks.
* **/public**: This is the folder that is generated by Hugo. This content needs to be published by you.
* **/static**: Any static files that need to be accessed by the site, e.g., CSS or JavaScript.
* **/package.json**: Node.js dependencies. Tailwind, for instance, is installed via the Node package manager (`npm`).
* **/config.toml**: Hugo's site configuration, like the root path and menu items. Hugo can access configuration elements when rendering the site. So you can define custom configuration settings here.
* **/syntax.css**: Hugo supports syntax highlighting via shortcodes. The highligher is configured via this CSS file.
* **/Makefile**: We use make to wrap some Hugo commands and to add addtional build steps.
* **/tailwind.config.js**: This is the Tailwind CSS framwork's configuration file.
* **/postcss.config.js**: Needed to make Tailwind statically accessible to the site.

## Build script and data files

The site can be built via `make all`. Here is what's executed:

1. Clean the current folder by deleting the generated output of a previous build
2. Install the necessary Node.js and Python dependencies
3. Build the components that were fetched from external Github repositories (e.g., client examples). This fetches source code files and generates data files.
4. Execute the `hugo` command to generate the HTML pages. The build result can be found within the folder `/public`

The command `make serve` performs the same steps, but serves the web pages on the local host for development purposes.

The `Makefile` contains details about the executed commands.

## Build pipeline

The build pipeline that is defined within `.github/workflows/main.yml` builds the documentation and uploads it into a GCS bucket:

1. Install Hugo
2. Check the branch out
3. Perform the build as stated before by using `make all`
4. Install the Google Cloud CLI
5. Authenticate to the GCS bucket
6. Validate the branch name
7. Sync the branch with a GCS folder
