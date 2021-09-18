# dtails-application

Welcome to Round 2 of the application process!
Our main goal with this test is to get a relevant idea of your programming skills, specifically HTML, CSS, and Javascript. Furthermore, the exercise will challenge you to learn a little bit of Shopify Liquid.

This will be a three step process:

**Step 1:** We will give you a few days to prepare with some relevant documentation links and setting up some tooling.

**Step 2:** You will choose a day that suits you to complete the test. Available days are the 16th to the 19th September.

**Step 3:** At *9.00 on the chosen day of your preference* we will send you an email with a link to our Github repo, which will contain a Readme with a full task description and the process for completion. You will fork the repo and have 12 hours to complete the task. That means that the deadline for completion is ***at 21.00 the same day***.  
We do not expect the task to take more than 4 hours, so the 12 hours is available in order for you to fit it into your day. We do expect regular commits throughout the task so that we can follow you work, but more on that later!  


## Preparation

You will be working directly on a Shopify theme, connected to a fully functioning Shopify store.
You will only have access to viewing your specific theme in the frontend,

To get started, the most important thing is for you to get aquainted with the Themekit CLI.
https://shopify.dev/themes/tools/theme-kit
Be aware that there is a new Shopify CLI for working with themes as well, but this is not relevant for our process at the moment.
Please read through the general documentation, especially https://shopify.dev/themes/tools/theme-kit/getting-started. 
Here you will be guided on how to install the CLI. However, you can safely ignore Steps 2-5. We will be providing you with a config.yml file in the repo, that will contain a Themekit password. You will only be responsible for updating the config file with a relevant Theme ID (which we will provide), and running the basic `theme watch` command.

Next, take a look at how Shopify's theme architecture looks: https://shopify.dev/themes/architecture.
You will not be asked to create new templates or sections, so this is just so you have a general idea of how a theme is structured.

And finally, for the fun part, you'll need to learn just a tiny bit of Liquid :) https://shopify.dev/api/liquid/basics.
The task will be focused on HTML, CSS, and Javascript, so we do not expect you to perform complex Liquid programming. But you will need to understand how to pull out some product data and variable assignment. Luckily, Liquid is quite straightforward, and the theme will have plenty of examples that you can look at.  
Checking out data objects beforehand could also be useful, specifically products: https://shopify.dev/api/liquid/objects/product

And that's it!

## Steps and process

 1. Fork this repo into your own Github account.
 2. Take the time to read the Task description below, as well as cloning the repo locally to your machine.
 3. Take a look around the theme and get comfortable with it.
 4. Make your first commit. **Important**: this marks the start of development. Other than the deadline at 21.00, you are not under any time constraints. However, after making this first commit, we do expect you to finish the task within a reasonable amount of time. Our estimate for this task is **4 hours**.
 5. Begin development! (see Theme development notes below)
 6. We would like to see a few regular commits along the way, at intervals that you find make sense. I.e. we appreciate commits that are readable and have a clear purpose, so give it some thought!
 7. Once you are satisfied with your results and have made your final commit, please submit a pull request to the main repo. **Imporant**: The pull request marks the completion of the task, and must be done before 21.00.
 8. You're done! Celebrate!

## Theme development

 1. In the email from us you will also have received a Theme ID. Replace the current `theme_id: 127267602623` in the `config.yml` file in the root directory of the theme.
 2. In your terminal, navigate to your theme repository.
 3. Run `theme open`. This will open a preview of the Shopify theme in your browser (you will notice at the bottom of the window it says "You’re previewing: ..."). If you accidentally close the preview, you can just run the command again. The preview link looks like this: https://dtails-application.myshopify.com?preview_theme_id=127267602623
 4. The storefront is also password protected, so once opened in the browser, you will be asked for a password. The password is *dtailsapplication*. You should only be asked for this once.
 5. Run `theme watch`
 6. Develop! It's really as simple as that, the process will watch for file changes in your theme, and upload them directly to Shopify for this specific theme. Reload the preview URL, and you will see your changes.

## Task description

Our focus will be on a collection page, located here: https://dtails-application.myshopify.com/collections/all
Therefore you do not have to apply any of this functionality to other templates/pages.

The task has two parts:

**Part 1: Secondary product image on mouse hover**

Each product in our shop has both a featured image and a gallery of extra images. Your job is to create a nice animation effect that will overlay the secondary image of the product when hovering over the element with a mouse. When the mouse leaves again, the featured image should switch back.
You are free to implement this using pure CSS, Javascript, or a combination of both.

**Part 2: API based currency conversion of product prices**

We would like for the customer to be able to switch the currency of the product prices with the click of a button.
The store currency is in DKK, and we would like to be able to display prices in USD.
For this shop, multi currency is not set up, so Shopify will not be able to deliver the data needed.

Therefore, using one of the many free online currency API's (your choice!), you will need to make an ajax call that retrieves an up-to-date currency exchange rate. You should then calculate and display the new price of each product in the new currency. So the current DKK price should be replaced by USD.

You should place the button somewhere on the collection page that makes sense. If you are up for the challenge, you could also place it in the navigation/header.

**Optional**: The user should be able to switch back to DKK as well. So you need to think of storing the DKK price data so that you can access it again.
You can therefore choose to use a select/dropdown to trigger the conversion. Thereby the user can easily switch back and forth by selecting one of the options.

___

Feel free to write inline CSS and Javascript. You are also welcome to create CSS and JS files and include them in theme.liquid, but it is not necessary.
You are welcome to use any framework/tool that might help you along the way. We do like vanilla JS though, so keep that in mind :)

For both tasks, focus only on desktop testing on a Chrome browser.

And as a final note - since this task is not overly complex, we are focusing mainly on your ability to write legible code and think creatively.     

## Hints

You should only need to make changes in two files.

 - `collection-template.liquid` for including your CSS and Javascript, and adding a button/selector for currency change
 - `product-card-grid.liquid` for editing the markup of the product items

___
Product images can be accessed via array indexes. So to grab the secondary image, you could do something like this:

     {%- if product.images.size > 1 -%}
	     {%- assign secondary_image = product.images[1] -%}
	     {%- assign secondary_img_url = secondary_image | img_url: '1x1' | replace: '_1x1.', '_{width}x.' -%}
	     // Markup for secondary image here
     {%- endif -%}

Don't worry too much about the fancy url manipulation. This is used for configuring lazyloading and autosizing. If you follow the pattern of the markup for the featured image, you should have no trouble getting it working.
___

Product prices are always stored in cents. So a product with a price of 100.00 DKK, when accessed via liquid (`{{product.price}}`) will return 10000. You don't need to think about different variant prices (i.e. price_min and price_max). Just use product.price.
The theme has a global JS helper, `formatMoney`, to format money for display. It can be accessed on the global `theme.Currency` object. It defaults to USD formatting, so you only need to pass the function money in cents.
