# Engine Theme Development Best Practices

Engine themes are designed with extensibility and longevity in mind. When developing, try to imagine how you would feel editing this theme two years from now. Keep in mind every Engine theme will eventually be inherited by the client.

These best practices are not hard and fast rules but instead are a guide in order for us to iterate quickly. Before you begin development, check out [Local Theme Development](local_theme_development.markdown).


## File Structure

```
theme_<theme_name>
├───engine_cms
│   └───posts
├───spree
│   ├───checkout
│		└───payment
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
│		└───favicons
│   ├───js
│		└───plugins
│   └───scss
│		└───plugins
└───README.md
```
## Liquid

1. All templates should be built with [liquid](https://shopify.github.io/liquid/basics/introduction/).
	- ERB is not ideal for engine because we don't want to get theme developers access to engine storefront models and controllers.
	- Liquid is the templating used for Shopify which makes the transition easier for lots of theme developers.
2. The shared folder should contain partials that are used more than once or if you believe it may be used more than once in the future, future developers will thank you.
3. Assets partials
	- scripts.liquid
	- head.liquid

## CSS

1. Bootstrap is the preferred framework
	- Why?
		- Adoption
			- Bootstrap is the most commonly used frontend framework, so future theme developers are more likely to be familiar with it.
		- Support
			- Bootstrap is supported by Twitter and a big community of front-end developers and documentation is well-written which makes answers easy to find.
2. Do's and Donut's
	- Do's
		- Use bootstrap grid and be mindful about naming selectors
		- Use variables_override.scss to override bootstrap variables
	- Donut's
		- Don't touch bootstrap source
		- Don't abuse inline styling. Most styling needs to be contained within the SCSS.
3. SCSS
	- We use SCSS mainly in order to keep our file structure semantic and clean.
	- [Compiling SCSS](local_theme_development.markdown).
	- File structure
		- style.scss contains global styles.
		- Each partial and page should have their own SCSS file.
		- variables_override.scss contains styles overriding bootstrap styles.

## Javascript

1. Don't use JS where CSS will work.
2. Use vanilla Javascript when possible and try to avoid using jQuery.
3. [Vue.js](https://vuejs.org/) is our framework of choice.
	- Vue is easy to pickup and we feel it's a good choice for longevity.
	- Vue can be implemented integrally.
	- Again, these rules aren't hard and fast if you feel another framework is the best choice for a particular project.
4. Global Javascript goes in custom.js.
5. Break partial Javascript out when possible.
