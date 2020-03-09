## Dangerously setting HTML and its XSS vulnerability in React

If you've ever worked in modern web development, at one stage, you may have required to input HTML directly into a component of your site. Although uncommon, in certain circumstances, whether it be due to a lack of functionality that a web package provides, or to inject styles for a component directly which you don't have access to otherwise, setting `innerHTML` through JavaScript can help take a shortcut to achieve what you need without the complexity of digging through source code, mangling packages together, or blowing up your app's bundle size.

Industry web frameworks usually discourage the use of setting `innerHTML`, with the concern that it opens up possibilities to **Cross Site Scripting (XSS)** attacks. XSS attacks involve injecting malicious code through the client-side of a website, allowing hackers to gain access to client secrets or to the server by hijacking user requests. By setting `innerHTML` without precaution, you are enabling users the ability to directly inject potentially harmful HTML or JS as they please.

React likes to remind developers of the dangers of injecting HTML directly by naming the HTML tag to `dangerouslySetInnerHTML`. By default, React is safe in design and makes it difficult to perform XSS attacks since all strings are *escaped* (i.e. sanitised from tags or injection). In fact, React acknowledges that when using this tag, you understand what you're doing; it disables checking the component for changes during its update lifecycle, which will bring minor performance improvements. Nevertheless, injecting custom HTML is straight forward in React.

```javascript
// Generates <span> tag with content
const generateHTML = () => {
  return {
    __html: "<span>Ooo, maybe this wasn't a good idea...</span>"
  };
}

// Injects generated span tag into div
const MyComponent = () => {
  return <div dangerouslySetInnerHTML={generateHTML()}/>;
}
```

Although appearing harmless at first, there are no bounds to what an attacker could inject into your code:

```javascript
const landingPageText = 
  "<button onClick={() => alert('Hacked!')}>",
  "Find our new site here!",
  "</button>";

const HomePage = () => {
  return (
  	<div 
      dangerouslySetInnerHTML={{
        __html: landingPageText.join()
	  }}
    />;
  );
}
```

Here is a simple example of inserting a button which, in turn, can gain access to client-side (and, with proper analysis, may be used to fetch server-side) data through exploitation of the `dangerouslySetInnerHTML` tag. Of course, an attacker with enough knowledge can use an XSS attack as such along with other security loopholes to compose a breach at a larger scale. [You can find a more practical example by Jacob Jang here, exploiting an input box to perform an XSS injection.](https://codesandbox.io/s/k9vxk9ppyo?autoresize=1&moduleview=1)

### Common uses for dangerously setting HTML

- Markdown rendering and styling
- Fetching data as raw HTML to embed into a site
- Server-side HTML page/component rendering
- Forcing styles on uneditable/inaccessible components

### Injecting HTML safely

In the case that you must directly inject HTML, there are workarounds to assure it is done safely. This is commonly done through input **sanitisation**. Sanitisation effectively filters through the injected content and strips out potentially malicious code, often by removing HTML tags or JS code that were intended to be utilised as strings - an example of abusing the [use-mention distinction](https://en.wikipedia.org/wiki/Use–mention_distinction). There are multiple packages that can be used to sanitise input in React, including [DOMPurify](https://github.com/cure53/DOMPurify), [js-xss](https://github.com/leizongmin/js-xss), [xss-filters](https://github.com/YahooArchive/xss-filters), and more. Most of these tools serve the same purpose, although some offer extended functionality such as restricting tag usage in certain inputs, restricting website references in HTML, or efficient performance that won't impact the speed of your app. Although sufficient in most circumstances, none of these packages are perfect - an attacker can analyse the source code carefully and may be able to exploit a weak entry point that can open up possibilities for XSS attacks post-sanitisation.

### Staying safe from XSS attacks

It is essential to stay safe from XSS attacks, regardless of whether you use the `dangerouslySetInnerHTML` tag in React. A few small steps can go a long way to stop attackers from gaining unauthorised access to critical data.

1. **Don't use the `dangerouslySetInnerHTML` tag.** It may be better to use external packages or to edit source code directly to achieve what you need instead. Attempt all other methods first before resorting to using it.

2. **Always sanitise user input.** Although React already provides sanitisation methods behind the scenes, it always helps to have an added layer on top. Use common open-source sanitisation packages to assure malicious code can't make it's way into your website.

3. **Follow security best practices.** Restricting access client-side will help limit what an attacker can do even if they successfully gain access through an XSS injection. Keep authorisation tokens safe and unreachable from the client-side. Encrypt data when interacting with the server. Sanitise the data server-side too and assure data is only processed if it comes from reliable sources you specify. The more layers of uncoupled security integrations, the harder you make each step for a hacker to breach your data.

4. **Use content headers when making HTTP requests.** Enforcing expected types in HTTP requests using the `Content-Type` and `X-Content-Type-Options` headers allows your server to assure the data you've requested/posted is the type its intended to be.

&nbsp;  
&nbsp;  

*Mozamel Anwary ~ March 2, 2020*