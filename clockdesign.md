## Summary

I wanted to build a 'dashboard' which showed a simple day, date and time - a bit like the Chromecast screensaver - using JavaScript, CSS, and HTML.

Along the way, I discovered how to make an international clock, how to use the query string, and how to position text over a background image.

Here's the final clock in desktop view:

![The Relax Clock](https://github.com/dfrancocci/bootstrap/blob/master/clockexample.png)

## Making the clock

JavaScript provides a method to convert a date object to local date/time string `dateObj.toLocaleDateString()`. There is also a 'toLocaleTimeString' method, although it seems like you can use either to format the date/time. I didn't explore what the differences were.

First I set up a number of DIVs to receive the day of the week, the date (day, month, year), and the time - as I wanted to style each of them separately:

    <div id='day'></div><br>
    <div id='date'></div><br>
    <div id='time'></div><br>
    
and then used the date and time methods to set the content of these DIVs:

    function displayClock() {
        let dateObj = new Date();
        day.textContent = dateObj.toLocaleDateString(locale, dayOptions);
        date.textContent = dateObj.toLocaleDateString(locale, dateOptions);
        time.textContent = dateObj.toLocaleTimeString(locale, timeOptions);
    };
    
and I put this inside a function, as you can see. Then to refresh the content, I used `setInterval` to call the function repeatedly every second:

    let t = setInterval(displayClock, refreshRate);
    
## Locale and date/time options

The date/time methods take a 'locale' and a set of options. The locale is essentially a language and country string which determines how the date/time will be formatted for language and international considerations. By default, the script sets this to 'en-GB':

    const defaultLocale = 'en-GB';
    
Here are the default date/time options used for the weekday, date, and time calls:

    const dayOptions = { 
        weekday: 'long',
    };
    const dateOptions = {  
        year: 'numeric', 
        month: 'long', 
        day: 'numeric', 
    };
    const timeOptions = {  
        hour: 'numeric', 
        minute: 'numeric',
        hour12: false
    };

You can read more about the locale and date/time options on the [Date.prototype.toLocaleDateString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString) page. 

I wanted to give the user the options to set the locale and also some of the options - 12 or 24 hour clock, and whether to display seconds. So, I used the query string to allow the user to set these options. Here is an example:

    ?locale=en-US&hour12=24&seconds=on
    
In this case:
- locale is set to 'en-US'
- 12 hour clock will be used (default is 24 hour clock)
- seconds will be shown (default is 'off')

To create an object from the query string, I used `URLSearchParams()` and then used the 'get' method on the resulting object to get the parameters I needed, for example:

    let urlParams = new URLSearchParams(window.location.search);
    let locale = urlParams.get('locale');
    
## Creating the background image

Back in the early days of website design, every site site seemed to have a background image - just because you could - usually with a repeating design, which often made the overlaying text difficult to read. 

Now, full page background images with overlaying text have been rediscovered - but the tutorials on how to do it seem to make heavy weather of it - using DIVs to hold the background image, and then further DIVs with absolute positioning to hold the text. 

I've used a simpler approach, which uses the default behaviour of text over a background image in the body. 

In the styling for the body, I added the following:

    <style>
        body {
            font-family: 'Jura', sans-serif;
            background-image: url('images/cat_in_chair.png');
            background-position: center;
            background-repeat: no-repeat;
            background-size: cover;
        }
        
In this case:
- `background-image` gives the URL for the image file
- 'center' - specifies the image should be centred - this makes a difference when the browser is shrunk beyond the size of the image - for example, on a phone
- 'no-repeat' stops the image from repeating, as the default is to repeat
- 'cover' ensures that the image covers the whole background of the browser window

Then the DIVs that I set up for the day, date, and time naturally appear over the top of the background image, without any further positioning required. To make the clock stand out, I've styled the text in white, and 900 font weight, and I've used the Jura font from Google fonts, which is a Eurostyle-like font.

## Putting it all together

You can check out the clock here: [Relax | Clock](http://dfrancocci.dx.am/clock.html).

All the code is on GitHub: [dfrancocci/site A](https://github.com/dfrancocci/siteA).