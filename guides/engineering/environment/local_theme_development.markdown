
# Developing themes locally

While we are building themes for our early adopters we will need to develop those themes locally. There are still some hiccups to work out, but generally speaking it's easy to work with themes in your local environment.

# Concerning theme development

Themes should *always* remain entirely de-coupled from both [engine_storefront](https://github.com/geminimvp/engine_storefront) and [solidus_liquid_themes](https://github.com/geminimvp/solidus_liquid_themes).

So themes are placed in their own repositories which can be loaded into your local environment via a rake task. Themes can be zipped up and loaded into a production site via the admin panel.

# Developing themes locally

1. Clone [engine_storefront](https://github.com/geminimvp/engine_storefront), you will need to have it [running locally](https://github.com/geminimvp/engine_storefront/blob/master/README.md) to host your themes.

	```
	$ git clone git@github.com:geminimvp/engine_storefront.git
	```

2. Clone the theme repository into the same directory you cloned [engine_storefront](https://github.com/geminimvp/engine_storefront).
	
	**The theme needs to be in the same local directory as the engine_storefront repository**
	<img src="https://media.giphy.com/media/l2JedPGiZueLYAryo/giphy.gif" />


	
	```
	$ git clone git@github.com:geminimvp/theme_THEME_NAME.git
	```

3. Create a branch in the theme repository to make your changes
	```
	$ git checkout -b BRANCH_NAME
	```
	- bug/branch-name... for small edits and visual bugs
	- feature/branch-name... for large changes and new site features
	- chore/branch-name... for housekeeping

4. [Run the theme:import rake task](https://github.com/geminimvp/engine_storefront/blob/master/README.md#importing-themes) to load the theme
	
	```
	$ THEME=THEME_NAME bundle exec rake theme:import
	```
	If you experience any issues with this step, refer to the [engine_storefront readme](https://github.com/geminimvp/engine_storefront/blob/master/README.md#importing-themes). Please add your issue to the caveats section if it is not already documented.

5. QA your changes locally

6. Commit and make a PR to the theme repository

# Deploying a theme 

## The safe way

Once your PR has been reviewed and merged, you can deploy to master via the [engine_storefront theme editor](https://github.com/geminimvp/engine_storefront/blob/master/app/models/spree/theme_archive_importer.rb).

Use the diff from the PR to find where changes need to be made in liquid and which theme_assets need to be refreshed. Complete this process manually, but make an effort to get as close as humanly possible to what we have in version control.

Due to the control our clients have over their own themes, it is unlikely that our theme repositories will ever be identical to what is in production, but it is important that we stay as close as possible. This will help us fix mistakes made in production and to minimize undocumented changes.

## The nuclear option

**Using this method will completely replace the existing theme, do not use this method unless you intend to reset the theme to exactly what is in GitHub. Take care not to erase client changes!**

 <img src="https://media.giphy.com/media/xUA7bbaSmCUfNYjhks/giphy.gif" />

Once your PR has been reviewed and merged, you can deploy to master via the [engine_storefront theme importer](https://github.com/geminimvp/engine_storefront/blob/master/app/models/spree/theme_archive_importer.rb).

1. After you have merged your changes, pull master.

	```
	$ git checkout origin master
	$ git pull
	```
	
2. Zip up the theme

	```
	$ zip -r theme_name.zip .
	```
	
3. Navigate to the **staging** admin panel and import a new theme from the theme_editor

4. QA on staging

5. Navigate to the **production** admin panel and import a new theme from the theme_editor
