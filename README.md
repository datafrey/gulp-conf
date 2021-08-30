# My gulp configuration

Here is my gulp configuration for work, hope someone will find it useful.

To use it:

1. Redact `package.json` according to your project data (name, author, etc.).

2. Open terminal/console in current directory and install required packages:

   ```console
   npm i
   ```

3. Initialize the project file structure using __one__ of these commands:

   ```console
   gulp init-project
   npm run init
   ```

   If you want to use SASS instead of SCSS change the `cssPreprocessorType` value among the variables at the beginning of the file to `'sass'` before running `init-project` task:

   ```javascript
   const cssDir = 'css'
   // const cssPreprocessorType = 'sass'     // uncomment this
   const cssPreprocessorType = 'scss' // comment this
   const distStylesPath = path.join(distPath, cssDir)
   const srcStylesPath = path.join(srcPath, cssPreprocessorType)
   ```

4. To start development server and watching tasks run __one__ of these commands:

   ```console
   gulp watch
   gulp
   npm run watch
   ```

5. To sync the `dist` folder content with the contents of `src` run this command (the `watch` task runs it every time at the beginnig):

   ```console
   gulp dist-sync
   ```

6. By default webpack compiles scripts in `src/js` folder in development mode while `watch` task is running. To make it do this in production mode run __one__ of these commands:

   ```console
   gulp build-prod-scripts
   npm run build-prod-scripts
   ```

## Project file structure

- /dist
  - /css
    - style.min.css
  - /fonts
  - /icons
  - /img
  - /js
    - bundle.js
    - bundle.js.map
  - index.html
- /node_modules
- /src
  - /fonts
  - /icons
  - /img
  - /js
    - /modules
    - main.js
  - /sass (or /scss)
    - /base
      - \_animations.sass (or \_animations.scss)
      - \_fonts.sass (or \_fonts.scss)
      - \_mixins.sass (or \_mixins.scss)
      - \_variables.sass (or \_variables.scss)
    - /blocks
    - /libs
    - style.sass (or style.scss)
  - index.html
- .eslintrc.json
- .gitignore
- .prettierrc.json
- gulpfile.js
- package-lock.json
- package.json

## Tasks description

### Common tasks

- `init-project` - creates project folders and files using variables in the start of the file.
- `dist-sync` - synchronizes `dist` folder's content with `src` folder's content (runs tasks: `html`, `styles`, `scripts`, `fonts`, `icons`, `images`).
- `watch` - starts development server on localhost:3000 by default and begins to watch the contents of `src` folders and update `dist` folders running development and resources tasks. This task is default for this gulpfile.

### Development tasks

- `html` - updates .html files in `dist` according to changes in `src`, removing whitespaces between elements.
- `styles` - compiles .sass/.scss files in `src/sass` / `src/scss` into `dist/css/style.min.css`, removing whitespaces.
- `scripts` - compiles scripts from `src/js` to `dist/js/bundle.js`, development mode.
- `build-prod-scripts` - compiles scripts from `src/js` to `dist/js/bundle.js`, production mode.

As entry point webpack uses `src/js/main.js`.

### Resources tasks

All tasks here compare files and directories in `src` and `dist` folders by name and change `dist`'s files and folders only if you rename their counterparts or add new in `src`. If you remove file or folder from `src`'s resources folders it will be removed from `dist`'s resources folders either.

- `fonts` - copies files from `src/fonts` to `dist/fonts`.
- `icons` - copies files from `src/icons` to `dist/icons`.
- `images` - minimizes images and copies them from `src/img` to `dist/img`.

  **imagemin** configuration:

  ```javascript
  imagemin([
    imageminPngquant({
      speed: 1,
      quality: [0.95, 1]
    }),
    imageminZopfli({
      more: true
    }),
    imageminGiflossy({
      optimizationLevel: 3,
      optimize: 3,
      lossy: 2
    }),
    imagemin.svgo({
      plugins: [{ removeViewBox: true }, { cleanupIDs: false }]
    }),
    imagemin.mozjpeg({
      quality: 90,
      progressive: true
    })
  ])
  ```
