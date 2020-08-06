
# GWIQ Tag Implementation Guide
### How Does It Work? What Are GWIQ Tags?

GWIQ tags are applied to your website or digital campaign to connect the GlobalWebIndex panel to your digital property. They make a call to the globalwebindex.net domain when a user visits your digital platform or is exposed to your campaign.

### How Do I Generate Tags?
GlobalWebIndex will issue unique tags for each campaign/website tracking. Please contact your account manager if you wish to begin tracking a new property.

### Tag Parameters
The following considerations are applicable for all versions of GWIQ tagging.

#### CampaignID (cid)
This will be supplied by GWI and is unique to the campaign being tracked. All elements of a single campaign must be tracked with the same CampaignID and a new CampaignID should be used for each new campaign tracked to avoid incorrect data attribution.

#### Key-Value Pairs
Key-value pairs are where additional information can be passed to GWI for segmentation. Only information passed to the tag will be available for segmentation.

Keys are the categories of data for segmentation. (e.g. if you would like to split by placement, creative or site, each category would be a different key).

Keys must be agreed with GWI before campaign launch. Keys added to the tag without approval are removed in data cleaning and cannot be recovered.

Values are the individual segments for the tag fire. (e.g. if creative is your key, you could use text like ‘MPU’ or ID codes like ‘123456’ as potential values.

Please inform GWI if you would like each value translated to more human-readable text in the platform. E.g. code 123456 -> ‘Retargeting Banner Concept 2’. If this is not provided, the value passed in the tag will be displayed in the platform as is.

#### Technical Requirements
Each key is unique within a tag fire
Keys must be lowercase
Alphanumeric
No spaces
No URL encoded characters
Length Limit: 64 characters per key and per value
When implementing the tags above be sure to implement any of the key-value pairs you would like to track. Many platforms offer the option of using ‘macros’, (Example - AppNexus) which can help automatically specify data about each placement when the tag fires. This makes it possible to segment multiple placements with a single tag implementation.

#### HTTP vs HTTPS
The default tag is created without a declared secured vs unsecured protocol. With the JavaScript tag, this will adopt the protocol used by the site at the time. If using the image pixel / URL tag, the tag should adopt the protocol of the site. However, some services require this to be explicitly declared. In these cases please prefix ‘http:’ or ‘https:’ before the ‘//gwiq’ in src of the tag.

### Tag Types
#### JavaScript - Asynchronous (Web Analytics)
The GWIQ JavaScript website tag can be implemented across your site. If you have implemented analytics tracking in the past, the format of the tag should be familiar. The tag should be implemented to fire on every event you wish to connect to the GlobalWebIndex panel.

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
Assuming your site is a single page application and it is initialized after browser window load event, you can call the ‘window.gwiq.track’ method any time during execution to track additional events.

If you need to track additional events before window load event, make sure the gwiq.js script is already loaded, as it is done in the example above.

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

Attention: be careful to disable debug mode during production usage, as it can affect performance and overall functionality. Exact features of debug mode are subject to change in the future.

##### Deprecated JS Tag
If you implemented GWIQ JS tag before, you probably used asyncjs method instead and passed parameters as a query string (e.g. key1=foo&key2=bar), as in the example below:

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
                gwiq.asyncjs('cid=%cid%&key=%value%');
            } catch (e) {}
        };
        var s = document.getElementsByTagName('script')[0];
        s.parentNode.insertBefore(js, s);
    })();
</script>
```

We recommend updating to use ‘track’ method, as it validates incoming parameters and can spot potential issues earlier.

#### Image Pixel `<img>` (Campaign Analytics)
The GWIQ image pixel is an impression tracking pixel that can be implemented across in browser placements. If you have implemented impression tracking in the past, the format of the tag should be familiar. The tag should be implemented to fire on every campaign exposure which connects that exposure passively to the GlobalWebIndex panel.

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

##### Where To Implement?
Ad Server / Exchange / DSP / Site Served Placements
The image pixel tag is compatible with all major Ad Servers/Exchanges, DSPs and many site served placements. The tag should be placed where there is an option to add an ‘impression pixel’ / ‘tracking pixel’ etc. This normally happens during the standard campaign trafficking process. Some services may simply request a URL to be used instead of a `<img>` tag, in these cases, use the URL tag format

##### Hard-Coding Into Creative
In cases where the above is not an option, the image pixel can be hard-coded into the creative when it is authored.

##### Websites
The tag should be placed in the `<head>` of the website alongside other analytics tags you may already use.

##### Tag Managers
The tag can also be delivered through a tag manager, such as Google Tag Manager, Ensighten or Tealium by using the code as a 'Custom HTML' element or equivalent. 

##### Technology Verification / Auditing
If the platform/site you are using requires third party tracking services to be verified/audited before tracking can be included in a campaign, GWI can provide evidence for existing verification. GWI can also go through new verification processes if required.

#### IAB TCF version 2.0 support

Globalwebindex is registered vendor on the IAB Global Vendor list with an id 536.

In order to validate users consensts against the TCF version 2.0 framework the img tag should look as follow:

```
<img src="https://gwiq.globalwebindex.net/gwiq/img/?cid=%cid%&key1=%value%&gdpr={GDPR}&gdpr_consent=${GDPR_CONSENT_536}" width="1" height="1" style="display:none"/>
```


## Still having trouble?
If you have any questions or difficulty implementing the tag, please contact your GWI Account Manager.
