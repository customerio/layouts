Layouts
=====

This repository contains the Foundation/Inky project where the starter layouts are built.  Using Inky to build email layouts is far easier than coding them from scratch.  It also makes the layouts responsive by "default".  

There are many manual steps for creating layouts to be used in the app.  Details for how to create a new layout or edit a new one to make available in the app are provided here. 

## **Foundation/Inky**
All layouts are created using the Foundation for Emails framework. Using Foundation for Emails enables us to build emails quickly without having to think about all the ins and outs of making them work in every email client and remain responsive. Learn more about Foundation here: https://foundation.zurb.com/emails/docs/

Inky is a templating engine built on top of Foundation that simplifies the HTML required to build an email. Emails are made up of many complex tables, but Inky provides simpler tags that make coding the email more intuitive. Learn more about Inky here: https://foundation.zurb.com/emails/docs/inky.html

The project in this repository is a Foundation for Emails (using SASS) project, started originally from their template and modified to meet our needs. Find more information about getting started here: https://foundation.zurb.com/emails/docs/sass-guide.html. However, the steps below are all you need to do to install and run the project

### Install and Run
- Clone this repo. 
- `cd layouts/layout_starters` 
- `npm start` - This will build the project (parse HTML and SASS) and start a server.  Your browser will open a new page automatically to the index.html file.

### File Structure
- `layout_starters/src/pages` - each starter's main Inky (html) file.
- `layout_starters/src/partials` - reusable sections of code. For example, every page has the same social link section, so it is defined as a partial and added into each page with `{{> social}}`
- `layout_starters/src/assets/scss` - SASS files for each starter.  Each starter has its own .scss file which is included in the `app.scss` file.
- `layout_starters/finals` - The final version of HTML and CSS that is inserted into `ui_api` to be used as the starter body code.  These are not automatically generated files.  They need to be updated manually with every change (more on this below).
- `layout_starters/dist` - folder where all generated HTML is placed after each build.  This is the HTML that should be copied and modified for CIO after each modification is made to the Inky files.

## **Development in Foundation/Inky Project**
### Styles
Each starter needs to have its own set of styles, independent from any of the other starters.  As such, when developing a new starter or working on an existing one, comment out all other page imports in the `app.scss` file.  Example for the `newsletter` starter, which contains the `social` partial:
```
@import 'settings';
@import 'foundation-emails';

/*Announcement*/
/*@import 'announcement';*/

/*Call To Action*/
/*@import 'call-to-action';*/

/*Menu*/
/*@import 'menu';*/

/*Newsletter*/
@import 'newsletter';

/* PARTIALS */
@import 'partials/social';
```
### Image Assets
If images are required for a starter, we need to make them publicly available for anyone who uses the starter in their layout.  To do this, use the image uploader in the email editor of CIO and copy the link to the image.  It will be something like: `http://userimg.customeriomail.com/B8vpKVBuS7mnEvYH2YRa_download_app_220.png`
The email that you use to upload the image does not have to remain live in order for the link to the image to work properly.

### Starter Card Image
Each starter card in the CIO app has a preview image.  

The image is a screenshot of the starter email running in the Foundation server.  As such, the Inky email should include the "Your Content Here" placeholder image:
```
<container class="content">
  <row>
    <columns small="12">
      <wrapper>
      	<!-- We'll replace this content tag with whatever you write in your email -->
        <img src="https://s3.amazonaws.com/fast.customer.io/email-templates/your_content_here.png">
      </wrapper>
    </columns>
  </row>
</container>
```
Once the email is built and the design is finalized, take a screenshot of the email running in the Foundation Server.  Resize the image to have a width of 600px (2x the width of the starter cards).

### Customization Comments
Be sure to add helpful comments to the layout for directing users about customizing certain areas.  Reference existing layouts for examples and to remain consistent in the language.

## **Modifications for CIO**
To prepare an email for use in the app, follow these steps:
- Ensure that only the import statements for the page you are working with are uncommented in `app.scss`.
- Build the Foundation project (if you have the server running, this will happen automatically).
- Copy the html from `layout_starters/dist`.  Paste into `layout_starters/finals`.
- Make the following manual changes to the HTML file in the `finals` directory:
    - Remove the link at the top for `app.css`.
    - Add the liquid link for the Foundation CSS (including the comment).
    ```
	  <!-- link to Foundation for Emails.  Removing or changing this link will break this layout.
    Learn more about Foundation for Emails and customizing their settings here - https://foundation.zurb.com/emails/docs/ -->
    {% include_foundation_css %}
    ```
    - Make sure there are no `href=undefined` links.  Replace all `undefined`s with `#`.  This replacement should be done in the Inky file as well to prevent the need to do it every time the HTML is built.
    - Replace the "Your Content Here" image with the `{{content}}` tag. 

## **Testing**
All layouts should be tested with Litmus.  The Litmus login is in 1Password.  Simply paste the HTML from the `layout_starters/finals` folder into Litmus and validate that everything appears consistently across all devices and email clients.  Any changes that need to be made in Litmus should be made to the original Inky files to ensure that they are propagated into future builds of the email.

## **ui_api**
Once the email has been coded, built, tested, and modified to function with CIO, it needs to be added to ui_api as a starter.  To do this:
- Copy the entire body to a new file in `ui_api/layouts/starter/<starter_name.go>`.
- Add the new starter to the list of starters in `ui_api/layouts/starter/starters.go`.

