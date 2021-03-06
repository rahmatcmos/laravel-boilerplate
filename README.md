# Laravel BS4 Vue CoreUI Boilerplate
> This is a Bootstrap 4 starter kit site with lite blogging feature, user account registration/management and full Vue CoreUI Backend based on Laravel 5.6, inspired by the popular [Laravel 5 Boilerplate](https://github.com/rappasoft/laravel-5-boilerplate). Unit & feature tests are not integrated yet, therefore this project isn't rock-solid for now. 

[![Build Status](https://drone.pc-world.fr/api/badges/adr1enbe4udou1n/laravel-boilerplate/status.svg)](https://drone.pc-world.fr/adr1enbe4udou1n/laravel-boilerplate)
[![StyleCI](https://styleci.io/repos/75558440/shield?style=flat&branch=master)](https://styleci.io/repos/75558440)
[![License](https://poser.pugx.org/adr1enbe4udou1n/laravel-boilerplate/license?format=flat)](https://packagist.org/packages/adr1enbe4udou1n/laravel-boilerplate)

## Demo

<p align="center">
<img src="https://user-images.githubusercontent.com/3679080/31575365-959dcec4-b0e5-11e7-9ddb-6902cf25b87a.gif">
</p>

* Frontend demo : [https://laravel-boilerplate.pc-world.fr](https://laravel-boilerplate.pc-world.fr)
* Backend demo : [https://laravel-boilerplate.pc-world.fr/admincp](https://laravel-boilerplate.pc-world.fr/admincp) (demo@example.com/demo, read-only)

## Features

### Frontend

* Bootstrap 4 Frontend with basic home-about-contact and legal mentions pages,
* [Slick carousel](http://kenwheeler.github.io/slick/) and [Cookie Consent](https://cookieconsent.insites.com/) integrated,
* Blog section, including navigation by tags & authors,
* [Intervention image](https://github.com/Intervention/image) for dynamic optimized images with cache plugin,
* [Turbolinks](https://github.com/turbolinks/turbolinks) included for fast navigation,
* Login throttle by recaptcha & [password strength meter](https://github.com/ablanco/jquery.pwstrength.bootstrap),
* Frontend user space and profile management. Email validation included. Registration can be disabled by environment parameter,
* [Laravel Socialite](https://github.com/laravel/socialite) included with all supported socialite providers (facebook/twitter/linkedin/github/bitbucket).

### Backend

#### Underlying layer

* Based on Bootstrap 4 [CoreUI](https://github.com/mrholek/CoreUI-Free-Bootstrap-Admin-Template) theme with many useful plugins ([SweetAlert2](https://limonte.github.io/sweetalert2/), [Flatpickr](https://chmln.github.io/flatpickr/), [CKEditor 5](https://ckeditor.com/), etc.),
* Entirely written with Vue components thanks to [Bootstrap-Vue](https://bootstrap-vue.js.org/), absolutely no jQuery dependency,
* Vue-route for instant client-side navigation,
* Native Vue Datatable, with everywhere search input and batch actions features,
* All main CRUD actions are ajaxified,
* Native [vue-select](https://github.com/sagalbot/vue-select) component for powerful select system (autocomplete, tags, etc.),
* Batch actions integrated within DataTables,
* Instant search engine (for posts) thanks to [Laravel Scout](https://github.com/laravel/scout) & [TNTSearch](https://github.com/teamtnt/tntsearch).

#### Features included

* User and permissions management (classic users <-> roles <-> permissions structure),
* Impersonation feature for quick user context testing,
* Frontend forms module, including settings (recipients and translatable message confirmation) & submissions management,
* Posts management for frontend blog, with granular publication permissions (classic draft-pending-published workflow). Posts include title, summary, html body, tags, featured image, metas. They can be published and/or unpublished at specific datetime and pinned if needed. Specific user can have limited access to his own posts only, according to his permissions,
* Wysiwyg drag & drop image upload.

### Localization & SEO

* Multilingual ready thanks to [Laravel Localization](https://github.com/mcamara/laravel-localization) package. Each routes are prefixed by locale in URL for best SEO support. For this boilerplate, EN & FR locales are 100% supported, including translated routes,
* Model Field Translatable support with [Laravel Translatable](https://github.com/dimsav/laravel-translatable), used for contact form confirmed message, metatags and posts,
* Robots and Sitemap integrated, including multilingual alternates,
* Full Metatags manager interface with translatable title & description. Meta entity can be either linked to route or specific entity like post,
* 301/302 redirections manager interface, with CSV/XLSX import feature.

### Developer Specific

* Usage of [Laravel-Mediable](https://github.com/plank/laravel-mediable) package for orderable media model management, used for featured image on posts,
* Permissions configuration based on config file rather than database,
* Form types defined on config file for settings & submission support. This boilerplate include just one "contact form" type,
* Custom webpack integration rather than laravel mix, for better flexibility (cf bellow),

## Install

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

1. `composer create-project --prefer-dist --stability=dev adr1enbe4udou1n/laravel-boilerplate my-new-project`
2. Set database and environment variables from **.env.example**
3. Set Web write permission if needed to `bootstrap/cache` and `storage` folders.
4. Launch follow commands :

### For Production :

```shell
composer install --no-dev --optimize-autoloader
php artisan key:generate
php artisan storage:link
php artisan migrate --force
```

### For Local/Development :

```shell
composer install
php artisan key:generate
php artisan storage:link
php artisan migrate [--seed]
```

### Initialize search index for posts

```shell
php artisan scout:import "App\Models\Post"
```

Laravel Scout takes care of updating posts index on CUD operations.

### Backend access

The first user to register will be automatically super admin with no restriction and will cannot be deletable.
Both frontend and backend have dedicated login pages.

## Development notes

### Compiling assets with Webpack

1. Install dependencies with `yarn`
2. Launch `yarn dev` for compiling assets and start dev-server with HMR enabled (preferred way for fast admin building)

> N1 : Use `yarn watch` if you prefer old school watcher  
> N2 : If assets modified, don't forget to launch `yarn prod` before deploy on each production environment.

### Permissions definitions

Unlike other known project as [ENTRUST](https://github.com/Zizaco/entrust) or [laravel-permission](https://github.com/spatie/laravel-permission), which are very well suited for generic roles/permissions, i preferred a more lite and integrated custom solution.

The mainly difference is that instead of store all permissions into specific SQL table, there are directly defined in a specific config file permissions. SQL side, roles entities relies only to a list of permissions key names.

Indeed i feel this approach better for maintainability simply because permissions are hardly tied to the application with Laravel Authorization. This is anyway the standard way in CMS as Drupal where each module have specific config permission file. Permissions should be only owned by developers.

### Note on Laravel Mix

You will observe that this boilerplate does not use [Laravel Mix](https://github.com/JeffreyWay/laravel-mix) which is shipped in Laravel for all assets management.

Laravel Mix still stay awesome for newcomers thanks to his laravel-like webpack fluent API, but, even if Laravel Mix can be easily overridden, for this project i preferred use my custom framework-free webpack setup in order to have total control of assets workflow.

For instance, with this custom setup HMR work natively with configurable port (essential for easy vue admin developpement) and productions assets are bundled into specific "dist" directory.

### Code styling

PHP-CS-Fixer & ESLint are used for strong style guidelines for both server and client side code.

PHP is pre-configured for official Laravel styling, just launch `./vendor/bin/php-cs-fixer fix` for global project auto-formatting.
  
JS use [JavaScript Standard Style](https://standardjs.com/) & eslint-loader is used within webpack for dynamic code styling recommendations.  
Moreover, [Official ESLint plugin for Vue.js](https://github.com/vuejs/eslint-plugin-vue) is included for heavy consistent code through all components vue files.

## TODO

- [x] <s>Data seeds</s>
- [x] <s>Batch actions</s>
- [x] <s>Form & menu access helpers</s>
- [x] <s>Metas management</s>
- [x] <s>Permissions management</s>
- [x] <s>Form submissions management</s>
- [x] <s>Client validation with vee-validate</s>
- [x] <s>301 redirection management with CSV/XLS import</s>
- [x] <s>Own account deletion</s>
- [x] <s>Account language & timezone selection</s>
- [x] <s>Account mail confirmation</s>
- [x] <s>Account avatar</s>
- [x] <s>Facebook/Twitter/Google Sign in with socialite package</s>
- [x] <s>Blog system (posts, publication date, multilangue, HTML wysiwyg, tags, featured image, medias, public user profile)</s>
- [x] <s>Dashboard</s>
- [x] <s>Switch to full Bootstrap 4 for both Frontend & CoreUI Backend</s>
- [x] <s>Migrate to 100% client-side Vue backend with vue-route</s>
- [x] <s>Migrate to Bootstrap-Vue</s>
- [x] <s>Webpack bundle size optimizations</s>
- [x] <s>Get rid of jquery datatables</s>
- [x] <s>Consistent VueJS components code styling</s>
- [ ] Inclusion of unit/featured/browser tests (stand by for now)

## License

This project is open-sourced software licensed under the [MIT license](https://adr1enbe4udou1n.mit-license.org).
