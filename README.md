# GWIQ Tag Implementation Guide

### Who is this guide for?
This guide is for people implementing a GWIQ tag on their website or advertisement. 
We will cover some [essential information for everyone](https://github.com/asifimranaziz/GWIQ-howto/edit/Update-instructions-for-agencies-1/README.md#how-does-it-work-what-are-gwiq-tags) about the tags we provide.

Second, we will cover [how parameters work](https://github.com/asifimranaziz/GWIQ-howto/Update-instructions-for-agencies-1/README.md#tag-parameters) in our tags

We then move on to the different types of ways to implement a tag depending on the technology being used to host it. 

- If you are an agency or ad trafficker refer to [this](https://github.com/asifimranaziz/GWIQ-howto/Update-instructions-for-agencies-1/README.md#tag-types-primaritly-for-ad-trafficers-and-agencies) section

- If you are a publisher refer to [this](https://github.com/asifimranaziz/GWIQ-howto/Update-instructions-for-agencies-1/README.md#tag-types-primaritly-for-publishers) section


##Essential information (for everyone)

### How Does It Work? What Are GWIQ Tags?
GWIQ tags are applied to your website or digital campaign to connect the GWI panel to your digital property. Provided the tag is implemented correctly,  the tag makes a call to the globalwebindex.net domain when a user visits your digital platform or a user is exposed to your advertising campaign.

The tags allow traffickers to ad platform [macros](https://github.com/asifimranaziz/GWIQ-howto/Update-instructions-for-agencies-1/README.md#macros---what-are-they) (DV 360, GAM, etc) if there is a need to track multiple placements under the same tag.

NOTE: It is ALWAYS recommended to test if a tag is working via a test page and if test data is being received by GWI. In order to ensure implementation/trafficking of the tag has been performed correctly.  

### How Do I Generate Tags?
GWI issues a unique tag for each campaign/website that needs tracking implemented. Contact your account manager if you wish to begin tracking a new property. 
  
## Tag Parameters
When GWI generates a tag the following items need to be considered for all versions of GWIQ tagging. 

#### CampaignID (cid)
This will be supplied by GWI and is unique to the GWIQ campaign being tracked. All elements of a single GWIQ campaign must be tracked with the same CampaignID. Data is attributed to the relevant CampaignID therefore a new CampaignID must be used for each new campaign being tracked.

NOTE: Making sure you use the correct cid avoids data being attributed to the wrong campaignID in GWIQ systems.

#### Key-Value Pairs

Key-value pairs, are added to our tags at the point of tag generation and allow additional information to be passed to GWI for segmentation.

NOTE: Key-Value Pairs must be agreed with GWI at the point of tag generation before a tag is implemented, and a campaign launches. Without approval, a key will be removed in data cleaning and cannot be recovered. Only information passed to the tag will be available for segmentation.

- Keys are the categories of data for segmentation. (e.g. if you would like to split by placement, creative or site, each category would be a different key within the tag to capture each of these pieces of data for each impression).

- Values are the individual segments that are sent to GWI when the tag fires. (e.g. if creative is your key, you could use text like ‘MPU’ or ID codes like ‘123456’ as potential values.) 

When we generate and send you a tag, we have used a placeholder for the values in each key-value pair which usually takes the form of %value%, etc. - you will need to replace each of these values placeholders with the appropriate macro used by your ad, or tag manager.

NOTE: Values are changed by the person implementing the tag on an ad platform, campaign or website. You can also use macros from a relevant platform if you wish. 

Let us take this example URL tag  

```
https://gwiq.globalwebindex.net/gwiq/img/?cid=ThisIsProvidedByGWI&key1=[EnterYourFirstValueHere]&key2=[EnterYourSecondValueHere]
```
 
Here are the cid and two key-value pairs, key1 and key2. 

 cid=ThisIsProvidedByGWI
 
 &
 
 key1=[EnterYourFirstValueHere]
 
 &
 
 key2=[EnterYourSecondValueHere]
 
 In this example you can see the cid has been provided to you by GWI it will look something like: 
 
 cid=c1027 
 
 For each of the 2 keys, the person trafficking must input a value or a macro. 
 After implementation they might look like this:
 
 key1=82374
 
 key2=footballadvert
 
 Or you might want to use macros for ease of implementation across multiple placements. If you were using DV360 they might look like this:
 
 key1=${CREATIVE_ID}
  
 key2=${CLICK_URL}

Macros can be added per the documentation of the platform being used. Here is DV360s macro documentation as an example:

https://support.google.com/displayvideo/answer/2789508?hl=en


In the example above the final tag that has been amended by a clients team to suit the platform serving it, DV360 in this case and placed on a campaign would look like this:

```
https://gwiq.globalwebindex.net/gwiq/img/?cid=c1027&key1=${CREATIVE_ID}&key2=${CLICK_URL}
```

NOTE: Make sure to create a test page for any trafficked tag to check the tag is firing and data is being received correctly. 

NOTE: The tag will fail to fire if any '%' characters are present in the tag that is loaded on the browser. 

Please inform GWI if you would like each value translated to more human-readable text in the platform. E.g. code 123456 -> ‘Retargeting Banner Concept 2’. If this is not provided, the value passed in the tag will be displayed in the platform as is. 

#### Technical Requirements
- Each key is unique within a tag fire
- Keys must be lowercase
- Each key should be alphanumeric
- There should be no spaces in each key
- There should be no URL-encoded characters in each key
- Length Limit: 64 characters per key and per value

When implementing the tags be sure to implement any of the key-value pairs you want to track. 

#### Macros - What are they?
As mentioned many platforms offer the option of using ‘macros’.Macros are placeholder strings in creative codes. The platform will replace the string with another value or code based on — the creatives, page, setting, etc. depending on the macro in use.

This helps automatically specify data about each placement when the tag fires. This makes it possible to segment multiple placements with a single tag.

Each platform has its own format for Macros, and you should replace our placeholders with the correct ones prior to trafficking. 
Example from Google Ads Manager: %%device%%,
Example from Adform: %%ADFCREID%%

Here are some links to popular platforms documentation on Macros
- Google Ads Manager (GAM) - https://support.google.com/admanager/answer/2376981?hl=en
- Display and Video 360 (DV360) - https://support.google.com/displayvideo/answer/2789508?hl=en


#### HTTP vs HTTPS
The default tag is created without a declared secured vs unsecured protocol. With the JavaScript tag, this will adopt the protocol used by the site at the time. If using the image pixel / URL tag, the tag should adopt the protocol of the site. However, some services require this to be explicitly declared. In these cases please prefix ‘http:’ or ‘https:’ before the ‘//gwiq’ in src of the tag.

## Tag Types primarily for Ad traffickers and Agencies

#### Image Pixel `<img>` (Campaign Analytics)
The GWIQ image pixel is an impression-tracking pixel that can be implemented across in browser placements. If you have implemented impression tracking in the past, the format of the tag should be familiar. The tag should be implemented to fire on every campaign exposure which connects that exposure passively to the GWI panel.

##### Where To Implement?
Ad Server / Exchange / DSP / Site Served Placements
The image pixel tag is compatible with all major Ad Servers/Exchanges, DSPs and many site-served placements. The tag should be placed where there is an option to add an ‘impression pixel’ / ‘tracking pixel’ etc. This normally happens during the standard campaign trafficking process. Some services may simply request a URL to be used instead of a `<img>` tag, in these cases, use the URL tag format

##### Example Image Pixel

```
<img src="https://gwiq.globalwebindex.net/gwiq/img/?cid=%cid%&key1=%value%" width="1" height="1" style="display:none"/>
```
 
URL (Campaign Analytics)
The URL tag can be useful for certain platforms that do not allow custom HTML/image tags to be used and instead require a URL which is automatically embedded within the creative when the ad is loaded.

##### Example URL Tag

```
https://gwiq.globalwebindex.net/gwiq/img/?cid=%cid%&key1=%value%
```

## Hard-Coding option
In cases where the above is not an option, the image pixel can be hard-coded into the creative when it is authored.


##### Websites
The tag should be placed in the `<head>` of the website alongside other analytics tags you may already use.

##### Tag Managers
The tag can also be delivered through a tag manager, such as Google Tag Manager, Ensighten or Tealium by using the code as a 'Custom HTML' element or equivalent. 

##### Technology Verification / Auditing
Suppose the platform/site you are using requires third-party tracking services to be verified/audited before tracking can be included in a campaign. In that case, GWI can provide evidence for existing verification. GWI can also go through new verification processes if required.


#### JavaScript - Asynchronous (Web Analytics)
The GWIQ JavaScript website tag can be implemented across your site. If you have implemented analytics tracking in the past, the format of the tag should be familiar. The tag should be implemented to fire on every event you wish to connect to the GWI panel.

Example JS Tag

```
<script type="text/javascript">
    (function () {
        var js = document.createElement('script');
        js.src = '//gwiqcdn.globalwebindex.net/gwiq/gwiq.js';
        js.type = 'text/javascript';
        js.async = true;
        js.onload = js.onreadystatechange = function () {
            var rs = this.readyState;
            if (rs && rs != 'complete' && rs != 'loaded') return;
            try {
                gwiq.track({
                    cid: '%cid%',
                    key: '%value%'
                });
            } catch (e) {}
        };
        var s = document.getElementsByTagName('script')[0];
        s.parentNode.insertBefore(js, s);
    })();
</script>
```

##### Single Page Application
Assuming your site is a single page application and it is initialized after the browser window load event, you can call the ‘window.gwiq.track’ method at any time during execution to track additional events.

If you need to track additional events before the window load event, make sure the gwiq.js script is already loaded, as it is done in the example above.

##### Debug Mode
During script implementation, advanced validation and logging could be useful. You can turn debug mode on by setting ‘gwiq.debug’ to true before calling ‘gwiq.track’ method. This will enable warnings about invalid parameters or method misuse in the browser console.

```
// Debug mode is enabled
gwiq.debug = true;
// This will track as usual
gwiq.track({
    cid: '%cid%',
    key: '%value%'
});
// This will not track, because CID is not passed. Also, this will output a warning into the browser console.
gwiq.track({
    key: '%value%'
});
```

Attention: be careful to disable debug mode during production usage, as it can affect performance and overall functionality. The exact features of debug mode are subject to change in the future.

#### IAB TCF version 2.0 support

GWI is a registered vendor on the IAB Global Vendor list with an id 536.

To validate user's consents against the TCF version 2.0 framework 2 parameters with the macros need to be added to the tag:

- gdpr with a macro {GDPR}
- gdpr_consent with a macro {GDPR_CONSENT_356}

The mentioned technical requirements for parameters are not valid for gdpr_consent parameter. It should be passed in the form which is generated by tcf 2.0 library.

The tag should look as follow:

```
<img src="https://gwiq.globalwebindex.net/gwiq/img/?cid=%cid%&key1=%value%&gdpr={GDPR}&gdpr_consent=${GDPR_CONSENT_536}" width="1" height="1" style="display:none"/>
```


## Still having trouble?
If you have any questions or difficulty implementing the tag, please contact your GWI Account Manager.

