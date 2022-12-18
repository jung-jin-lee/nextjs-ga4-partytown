# nextjs-ga4-partytown

This is sample ga4 to partytown

`_app.tsx`

```jsx
import '../styles/globals.css';
import type { AppProps } from 'next/app';
import { Partytown } from '@builder.io/partytown/react';
import Script from 'next/script';

export default function App({ Component, pageProps }: AppProps) {
  return (
    <>
      <Partytown forward={['dataLayer.push', 'gtag']} />
      <Script
        async
        src={`https://www.googletagmanager.com/gtag/js?id=${process.env.NEXT_PUBLIC_GA_MEASUREMENT_ID}`}
        strategy="worker"
      ></Script>
      <Script id="gtag-init" type="text/partytown">
        {`
          window.dataLayer = window.dataLayer || [];
          window.gtag = function gtag(){dataLayer.push(arguments);}
          gtag('js', new Date());
          gtag('config', '${process.env.NEXT_PUBLIC_GA_MEASUREMENT_ID}');
        `}
      </Script>
      <Component {...pageProps} />
    </>
  );
}
```

`next.config.js`

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  experimental: {
    nextScriptWorkers: true,
  },
};

module.exports = nextConfig;
```
