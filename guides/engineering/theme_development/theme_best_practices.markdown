# Engine Theme Development Best Practices

Engine themes are designed with extensibility and longevity in mind. When developing a new theme, think about how you would feel extending the theme two years from now. Keep in mind every Engine theme will eventually be inherited by the client.

These best practices are not hard and fast rules; instead are guidelines to help us iterate quickly. Before you begin development, read [Local Theme Development](local_theme_development.markdown).

<img src="https://toggl.com/blog/wp-content/uploads/2017/02/software-developer-life-cycle-toggl-blog-cover.jpg" width="500">

This is meant to be a living document, please make share your knowledge with the team by submitting a PR.


## File Structure

```
theme_<theme_name>
├───engine_cms
│   └───posts
├───spree
│   ├───checkout
│   |	└───payment
│   ├───home
│   ├───layouts
│   ├───orders
│   ├───products
│   ├───shared
│   └───taxons
├───theme_assets
│   ├───css
│   ├───fonts
│   ├───img
│   |	└───favicons
│   ├───js
│   |	└───plugins
│   └───scss
|	└───plugins
└───README.md
```

## Version Control

As this is a startup some cowboy coding is expected, but it is never a good idea to ignore version control. During the development and support of any theme you build, keep your code up to date in GitHub. Even if you think a fix is temporary or insignificant, make a PR and document that you made the change.

*If you write code outside of version control, that code doesn't exist.*

## Liquid

Liquid is a templating language first developed by Shopify. We use it in place of HTML or ERB to create the markup for our themes. [Liquid is open source](https://github.com/Shopify/liquid) and easy to extend with Drops.

1. All templates should be built with [Liquid](https://shopify.github.io/liquid/basics/introduction/).
	- ERB is not ideal for Engine themes because theme developers should not have access to `engine_storefront` models.
	- Liquid is the templating used in Shopify themes; removing a barrier for brands looking to switch to Engine.
2. The shared folder should contain partials that could be used in more than one part of the site
	- When in doubt, make it a partial! Future developers will thank you.
3. Assets partials
	- scripts.liquid
	- head.liquid

## CSS

1. [Bootstrap](https://getbootstrap.com/) is Engine's preferred CSS framework
	- Why? (Glad you asked)
		- Adoption
			- Bootstrap is the most commonly used front-end framework, so future theme developers are more likely to be familiar with it (remember we care about longevity).
			- Bootstrap has been tested on thousands of sites across the web.
		- Support
			- Bootstrap is supported by Twitter and a massive community of front-end developers.
			- The documentation is well-written which makes answers easy to find.
			- It is rare that the answer to your problem isn't on Stackoverflow
2. Do's and Donut's 🍩
	- Do's
		- Use the Bootstrap grid and be mindful about naming selectors
		- Use variables_override.scss to override Bootstrap variables
	- Donut's
		- Don't touch Bootstrap source, it is confusing (and mean).
		- Don't abuse inline styling. Most styling needs to be contained within the SCSS.
3. SCSS
	- We use SCSS mainly in order to keep our file structure semantic and clean.
	- [Compiling SCSS](local_theme_development.markdown). SCSS is compiled into one style.css file. style.css should not be edited because it will be overwritten when SCSS is compiled.
		- From the directory containing style.scss: `sass --sourcemap=none --watch style.scss:../css/style.css`
	- File structure
		- `style.scss` should contain global styles. Consider building a CSS "brand Bible."
		- Each partial and page should have it's own SCSS file.
		- variables.scss contains variables used or will be used in more than one SCSS file.

## Javascript

1. Don't use JS where CSS will work.
2. Use vanilla Javascript when possible and try to avoid using jQuery.
	- **Please** [use ES6](https://github.com/getify/You-Dont-Know-JS/tree/master/es6%20%26%20beyond)
3. [Vue.js](https://vuejs.org/) is Engine's framework of choice.
	- Vue is easy to pickup and we feel it's a good choice for longevity.
	- Vue can be implemented incrementally, which is ideal for themes which might be inherited by novice developers.
	- If you feel strongly that Vue is not the best tool for your theme, don't use it.
4. Global JavaScript goes in `custom.js`.
5. Break JavaScript into smaller files when possible.
