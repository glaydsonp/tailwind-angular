# Angular app integrated with TailwindCSS

# From start

Install tailwind
1 - npm i tailwindcss

Install new webpack and postcss as dev-dependecies
2 - npm i -D @angular-builders/custom-webpack @fullhuman/postcss-purgecss

Init TailwindCSS
3 - npx tailwind init

4 - Create a new scss file: src/tailwind.scss
5 - Add this to the scss file:

@tailwind base;
@tailwind components;
@tailwind utilities;

6 - At the root of the project, create the file "extra-webpack.config.js"
7 - Add this to it:

const purgecss = require('@fullhuman/postcss-purgecss')({
  // Specify the paths to all of the template files in your project
  content: ['./src/**/*.html', './src/**/*.component.ts'],
  // Include any special characters you're using in this regular expression
  defaultExtractor: content => content.match(/[\w-/:]+(?<!:)/g) || []
});

module.exports = (config, options) => {
  console.log(`Using '${config.mode}' mode`);
  config.module.rules.push({
    test: /tailwind\.scss$/,
    use: [
      {
        loader: 'postcss-loader',
        options: {
          plugins: [
            require('tailwindcss')('./tailwind.config.js'),
            require('autoprefixer'),
            ...(config.mode === 'production' ? [purgecss] : [])
          ]
        }
      }
    ]
  });
  return config;
};

8 - Config angular.json
8.1 - Stage changes to build
8.1.1 - ng config projects.<my-project>.architect.build.builder @angular-builders/custom-webpack:browser
8.1.2 - ng config projects<my-project>.architect.build.options.customWebpackConfig.path extra-webpack.config.js
8.2 - Stage changes to serve
8.2.1 - ng config projects.<my-project>.architect.serve.builder @angular-builders/custom-webpack:dev-server
8.2.2 - ng config projects<my-project>.architect.serve.options.customWebpackConfig.path extra-webpack.config.js

9 - Add tailwind.scss to file styles array
9.1 - ng config projects.<my-project>.architect.build.options.styles[1] src/tailwind.scss

DONE! 
