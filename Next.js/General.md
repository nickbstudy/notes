# Next.JS

[Position in learning](https://nextjs.org/learn/basics/data-fetching/implement-getstaticprops)

Prerenders pages, moving as much logic to HTML and away from JS as possible, enhancing performance and SEO.  When a page is loaded its JS runs and makes the page fully interactive (this process is called 'hydration').

---

### Link

All pages (except index.js) go in a folder called `pages` in the root folder, then you can `import Link from 'next/Link'` and use the `<Liink href="/page">` syntax instead of an `<a>` tag.  This preloads the pages and keeps them as part of a SPA.

---

### Image

`import Image from 'next/image'` gives access to the `<Image src="/images/name.jpg" height={100} width={120} alt="Alt.">` tag.  Images should all go in root/public/images.  The advantage of this over a standard `<img>` is loading on-demand, meaning performance isn't penalized for images outside of the viewport.

---

### CSS

Create a file with the same name as the component/page you want to style, but instead of `.js` end it with `.module.css` and in the js file import using `import styles from './filename.module.css'` then className must be `className={styles.whatever}` or for multiples you can use an array and a join: `className={[styles.st1, styles.st2].join(" ")}`  
An advantage of this is ensuring no name collisions will happen.

To add global state create a file called `pages/_app.js` with the following content:
```
export default function App({ Component, pageProps }) {
    return <Component {...pageProps} />;
}
```
You will need to restart npm after this.  Create a file to use for all css (suggests using `/styles/global.css`) and at the top of `_app.js` goes `import '../styles/global.css'`.  This is the **only** place you can import a global css file.

**clsx** can also make life easier in switching classes

---

### Pre-rendering and Data Fetching

https://nextjs.org/learn/basics/data-fetching/implement-getstaticprops
