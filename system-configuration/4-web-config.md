# Configuring the Front End Web UI


## Web UI configuration

Start by creating a fork of the Influenzanet [participant-webapp](https://github.com/influenzanet/participant-webapp) repository in your local organization or create a local copy of this repository.

The participant webapp is a configurable front end that relies on ```json```/```ts``` configurations to dynamically adapt the layouts of the different pages on the user interface. The confugrations are split into two main parts:

1. Application level config: Contained in ```public/assets/configs```. Consists on application level settings like languages supported, header, footer, and page layouts.

2. Locale level config: Contained within ```public/assets/locales```. Consists of language specific labels organised according to the different pages set in the application level config.

To get started, the example-public-folder contains example configurations within the assets folder. Create a copy of this folder and rename it to ```public```. Then, perform the following changes: 

1. Update the ```public/assets/locales``` to add folders for each supported language.

2. Configure the keys, markdown content within the locales folders.

3. Upload additional images needed into the ```public/assets/images``` folder.

4. Configure page layouts, headers, and footers by configuring the files present in ```public/assets/configs```.

    4.1. **Appconfig**: Set the language codes for the supported languages here. Also configure the avatars available for user profiles here.

    4.2. **header**: Set the logo image and styles used in the header here.

    4.3. **Navbar**: Set the items and the URL's they map on the navigation bar. This also includes the items that show up under the user dropdown on the right corner of the navbar.

    4.4. **Footer**: Configure the columns and links in the footer section.

    4.5. **Pages**: Contains an array of the different pages of the webapp. Here you can map a url to a page, and set the layout, contents, elements and styles for each page item. 
    
    **NOTE**: "pageKey" is used to map to a file in locales to render the labels for the elements of the page in each language. "itemkey" defines the name to be looked for in the file defined by "pageKey".

**Next**: [Configuring the mail server](../system-configuration/5-mailing-config.md)
