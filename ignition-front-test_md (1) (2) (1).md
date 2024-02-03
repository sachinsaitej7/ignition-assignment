# Ignition | Frontend assignment

## 1
Create function `dscount` that returns the number of successive symbols in a string, case-insensitive

```js
function dscount(str, s1, s2) {
  const regexp = new RegExp(`${s1}(?=(${s2}))`, "gi");
  const result = str.match(regexp);
  return result !== null ? result.length : 0;
}
```

This function should pass following tests:
```js
"use strict";

try {
    test(dscount, ['ab___ab__', 'a', 'b'], 2);
    test(dscount, ['___cd____', 'c', 'd'], 1);
    test(dscount, ['de_______', 'd', 'e'], 1);
    test(dscount, ['12_12__12', '1', '2'], 3);
    test(dscount, ['_ba______', 'a', 'b'], 0);
    test(dscount, ['_a__b____', 'a', 'b'], 0);
    test(dscount, ['-ab-аb-ab', 'a', 'b'], 2);
    test(dscount, ['aAa', 'a', 'a'], 2);

    console.info("Congratulations! All tests passed.");
} catch(e) {
    console.error(e);
}

function test(call, args, count, n) {
    let r = (call.apply(n, args) === count);
    console.assert(r, `Found items count: ${count}`);
    if (!r) throw "Test failed!";
}
```

## 2
1. What does this function do?
Ans. This function returns the last index of the last occurrence of a or b in the string s. If a or b is not found, it returns -1.

2. How would you improve it?
Ans. I would improve it by adding type guards, string in-built methods (better readability) and removing the redundant if statements.

```js

function func(s, a, b) {
  if (typeof s !== 'string' || typeof a !== 'string' || typeof b !== 'string') return -1;

  let aIndex = s.lastIndexOf(a);
  let bIndex = s.lastIndexOf(b);
  return aIndex > bIndex ? aIndex : bIndex;
}

```

```js
function func(s, a, b) {

	if (s.match(/^$/)) {
		return -1;
	}
	
	var i = s.length -1;
	var aIndex =     -1;
	var bIndex =     -1;
	
	while ((aIndex == -1) && (bIndex == -1) && (i > 0)) {
	    if (s.substring(i, i +1) == a) {
	    	aIndex = i;
    	}
	    if (s.substring(i, i +1) == b) {
	    	bIndex = i;
    	}
	    i = i - 1;
	}
	
	if (aIndex != -1) {
	    if (bIndex == -1) {
	        return aIndex;
	    }
	    else {
	        return Math.max(aIndex, bIndex);
	    }
	}
	
	if (bIndex != -1) {
	    return bIndex;
	}
	else {
	    return -1;
	}
}
```

## 3
Improve following code
```js

function calculateRating(vote) {
    const rating = Math.ceil(vote / 20);
    return rating;
};

function drawRating(vote) {
    const rating = calculateRating(vote);
    switch (rating) {
        case 0:
        case 1:
            return '★☆☆☆☆';
        case 2:
            return '★★☆☆☆';
        case 3:
            return '★★★☆☆';
        case 4:
            return '★★★★☆';
        case 5:
            return '★★★★★';
    }
}

function drawRating(vote) {
	const 
	if (vote >= 0 && vote <= 20) {
    	return '★☆☆☆☆';
	}
	else if (vote > 20 && vote <= 40) {
		return '★★☆☆☆';
	}
	else if (vote > 40 && vote <= 60) {
		return '★★★☆☆';
	}
	else if (vote > 60 && vote <= 80) {
		return '★★★★☆';
	}
	else if (vote > 80 && vote <= 100) {
		return '★★★★★';
	}
}

console.log(drawRating(0) ); // ★☆☆☆☆
console.log(drawRating(1) ); // ★☆☆☆☆
console.log(drawRating(50)); // ★★★☆☆
console.log(drawRating(99)); // ★★★★★
```

## 4
Create function `parseUrl` that parses URL and returns parsed data
```js

function parseUrl(urlString) {
    const { href, hash, port, host, protocol, hostname, pathname, origin } = new URL(urlString);
    return { href, hash, port, host, protocol, hostname, pathname, origin };
}

let a = parseUrl('http://haveignition.com:8080/fizz/buzz.css?a=1&b[]=a&b[]=b#foo')

console.log( a.href == "http://haveignition.com:8080/fizz/buzz.css?a=1&b[]=a&b[]=b#foo" )
console.log( a.hash == "#foo" )
console.log( a.port == "8080" )
console.log( a.host == "haveignition.com:8080" )
console.log( a.protocol == "http:" )
console.log( a.hostname == "haveignition.com" )
console.log( a.pathname == "/fizz/buzz.css" )
console.log( a.origin == "http://haveignition.com:8080" )


```

## 5
Share with us example of your React code

```jsx
import React from "react";

import Toast from "../Toast";
import styles from "./ToastShelf.module.css";
import { ToastContext } from "../ToastProvider";

const useEscapeKey = (handler) => {
  React.useEffect(() => {
    const handleEscape = (e) => {
      if (e.key === "Escape") {
        handler();
      }
    };
    window.addEventListener("keydown", handleEscape);
    return () => window.removeEventListener("keydown", handleEscape);
  }, []);
};

function ToastShelf() {
  const { messages, setMessages } = React.useContext(ToastContext);
  useEscapeKey(() => setMessages([]));

  const handleDeleteMessage = (id) => {
    setMessages((prev) => {
      return prev.filter((m) => m.id !== id);
    });
  };

  return (
    <ol className={styles.wrapper}>
      {messages.map((message) => {
        return (
          <li key={message.id} className={styles.toastWrapper}>
            <Toast
              type={message.type}
              message={message.value}
              onClose={() => handleDeleteMessage(message.id)}
            />
          </li>
        );
      })}
    </ol>
  );
}

export default ToastShelf;