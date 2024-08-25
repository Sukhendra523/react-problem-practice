# React problems to practice for frontend developer interviews:

<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Problem 1: Character Counter](#problem-1-character-counter)
- [Problem 2: Toggle Visibility](#problem-2-toggle-visibility)
- [Problem 3: Fetch and Display Data](#problem-3-fetch-and-display-data)
- [Problem 3.1: Implement caching HTTP requests](#problem-31-implement-caching-http-requests)
- [Problem 4: Debounced Search](#problem-4-debounced-search)
- [Problem 4.1 : Debounce and Reset input value](#problem-41-debounce-and-reset-input-value)
- [Problem 5: Form Validation](#problem-5-form-validation)
- [Problem 6.1: Countdown Timer](#problem-61-countdown-timer)
- [Problem 6.2: Countdown Timer with start and stop button](#problem-62-countdown-timer-with-start-and-stop-button)
- [Problem 7: Dynamic List](#problem-7-dynamic-list)
- [Problem 8: Controlled Tabs](#problem-8-controlled-tabs)
- [Problem 8.2: Dynamic Routes Tabs Navigations](#problem-82-dynamic-routes-tabs-navigations)
- [Problem 9: Light/Dark Mode Toggle](#problem-9-lightdark-mode-toggle)
- [Problem 10: Modal Popup](#problem-10-modal-popup)
- [Problem 11: Infinite Scroll](#problem-11-infinite-scroll)
- [Problem 12: Sorting a List](#problem-12-sorting-a-list)
- [Problem 13: Drag and Drop](#problem-13-drag-and-drop)
- [Problem 14.1: Accordion](#problem-141-accordion)
- [Problem 14.2: Accordion](#problem-142-accordion)
- [Problem 15: Editable Table](#problem-15-editable-table)
- [Problem 16: Star Rating](#problem-16-star-rating)
- [Problem 17: Weather App](#problem-17-weather-app)
- [Problem 18: Timer with Start/Stop/Reset](#problem-18-timer-with-startstopreset)
- [Problem 19: Dynamic Form Fields](#problem-19-dynamic-form-fields)
- [Problem 19.2: Dynamic Config Driven Form](#problem-192-dynamic-config-driven-form)
- [Problem 20: Notification System](#problem-20-notification-system)
- [Problem 21: Traffic Light](#problem-21-traffic-light)
- [Problem 22: Create Google calculator app](#problem-22-create-google-calculator-app)
- [Problem 23: Selectable grid](#problem-23-selectable-grid)
- [Problem 24: Multi-Select-Search](#problem-24-multi-select-search)
- [Problem 25: Pagination](#problem-25-pagination)
- [Problem 25: Smart-Search](#problem-25-smart-search)
- [Problem 26: Stepper](#problem-26-stepper)
- [Other Repos for react coding challenges](#other-repos-for-react-coding-challenges)

<!-- TOC end -->

<!-- TOC --><a name="problem-1-character-counter"></a>

### Problem 1: Character Counter

- **Description:**
  - Create an input box and a paragraph tag under this input box.
  - As the user types in the input box, the paragraph tag should display the number of characters typed.
  - When the character count exceeds 10, the text color in the paragraph tag should turn red.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    import { useState } from "react";
    import './styles.css'
    
    const CharacterCount = ({limit=10}) => {
        const [totalCharacterCount, setTotalCharacterCount] = useState(0);
        const [error, setError] = useState(false);
    
        const handleChange = ( e ) =>{
            const totalTextLength = e.target.value.length; 
            setTotalCharacterCount(totalTextLength);
            setError(totalTextLength > limit);
        }
    
      return (
        <div>
            <input type="text" onChange={handleChange}/>
            <p className={`${error?'error':''}`}>{totalCharacterCount}</p>
        </div>
      )
    }
    
    export default CharacterCount;
    ```
    
    ```css
    .error{
      color: red;
    }
    ```
</details>

<!-- TOC --><a name="problem-2-toggle-visibility"></a>

### Problem 2: Toggle Visibility

- **Description:**
  - Create a button that toggles the visibility of a paragraph.
  - Initially, the paragraph should be visible.
  - When the button is clicked, the paragraph should be hidden, and the button text should change to "Show".
  - When the button is clicked again, the paragraph should be shown, and the button text should change to "Hide".

<details>
  <summary> Solution: </summary>
  
    - **1st**
        
        ```jsx
        import { useState } from "react";
        
        const VisibilityToggle = () => {
          const [showParagraph, setShowParagraph] = useState(true);
        
          const toggleVisiblity = () => setShowParagraph(!showParagraph);
          return (
            <div>
              {showParagraph && <p>Paragraph text</p>}
              <button onClick={toggleVisiblity}>
                {showParagraph ? "Hode" : "Show"}
              </button>
            </div>
          );
        };
        
        export default VisibilityToggle;
        ```
        
    - **2nd Reusable.**
        
        ```jsx
        
        import { useState } from "react";
        
        const VisibilityToggle = ({ children }) => {
          const [show, setShow] = useState(true);
        
          const toggleVisiblity = () => setShow(!show);
          return (
            <div>
              {show && children}
              <button onClick={toggleVisiblity}>{show ? "Hide" : "Show"}</button>
            </div>
          );
        };
        
        export default VisibilityToggle;
        
        ```
</details>

<!-- TOC --><a name="problem-3-fetch-and-display-data"></a>

### Problem 3: Fetch and Display Data

- **Description:**
  - Create a button that, when clicked, fetches data from an API and displays it in a list.
  - Use a placeholder API like [[JSONPlaceholder](https://jsonplaceholder.typicode.com/)](https://jsonplaceholder.typicode.com/).
  - Display a loading message while fetching the data and handle any potential errors gracefully.

<details>
  <summary> Solution: </summary>
    - 1st without custom hook
        
        ```jsx
        // PostContainer
        // States = > posts , loading , error
        // fetch post on mount
        
        import React, { useEffect, useState } from "react";
        
        const PostContainer = () => {
          const [posts, setPosts] = useState([]);
          const [loading, setLoading] = useState(false);
          const [error, setError] = useState(false);
        
          const fetchPosts = async () => {
            setLoading(true);
            try {
              const response = await fetch(
                "https://jsonplaceholder.typicode.com/posts"
              );
              if (response.ok) {
                const data = await response.json();
                setPosts(data);
              }
            } catch (error) {
              setError(
                error.message ||
                  error.body.message ||
                  "Sorry! something went wrong. please try again"
              );
              console.error(error);
            } finally {
              setLoading(false);
            }
          };
        
          useEffect(() => {
            fetchPosts();
          }, []);
        
          return (
            <div>
              {loading && <span>Loading....</span>}
              {!loading &&
                posts.map(({ title, body, id }) => (
                  <div key={id}>
                    <h1>{title}</h1>
                    <p>{body}</p>
                  </div>
                ))}
              {error && !loading && <p>{error}</p>}
            </div>
          );
        };
        
        export default PostContainer;
        ```
        
    - 2nd with custom hook
        
        ```jsx
        
        import { useCallback, useEffect, useState } from "react";
        const defaultOptions = { method: "GET" };
        
        const useFetchData = (url, initialData=[], options = defaultOptions) => {
          const [loading, setLoading] = useState(false);
          const [error, setError] = useState(false);
          const [data, setData] = useState(initialData);
        
          const fetchData = useCallback(async () => {
            setLoading(true);
            try {
              const response = await fetch(url, options);
        
              if (response.ok) {
                const responseData = await response.json();
                setData(responseData);
              }
            } catch (error) {
              setError(
                error.message ||
                  error.body.message ||
                  "Sorry something went wrong please again later!"
              );
              console.error(error);
            } finally {
              setLoading(false);
            }
          }, [url, options]);
        
          useEffect(() => {
            fetchData();
          }, [fetchData]);
        
          return { loading, data, error };
        };
        
        export default useFetchData;
        
        ```
        
        ```jsx
        /* 
        
        - Create a button that, when clicked, fetches data from an API and displays it in a list.
        - Use a placeholder API like [JSONPlaceholder](https://jsonplaceholder.typicode.com/).
        - Display a loading message while fetching the data and handle any potential errors gracefully.
        
        */
        
        // PostContainer
        // States = > posts , loading , error
        // fetch post on mount
        
        import useFetchData from "./Hook/useFetchData";
        
        const PostContainer = () => {
          const {
            loading,
            data: posts,
            error,
          } = useFetchData("https://jsonplaceholder.typicode.com/posts");
        
          return (
            <div>
              {loading && <span>Loading....</span>}
              {posts.map(({ title, body, id }) => (
                <div key={id}>
                  <h1>{title}</h1>
                  <p>{body}</p>
                </div>
              ))}
              {error && !loading && <p>{error}</p>}
            </div>
          );
        };
        
        export default PostContainer;
        
        ```
</details>

<!-- TOC --><a name="problem-31-implement-caching-http-requests"></a>

### Problem 3.1: Implement caching HTTP requests

<details>
  <summary> Solution: </summary>

```jsx
// cache.js
const cache = {};

export const getFromCache = (key) => {
  return cache[key];
};

export const setToCache = (key, data) => {
  cache[key] = data;
};
```

```jsx
// useCachedFetch.js
import { useState, useEffect } from "react";
import { getFromCache, setToCache } from "./cache";

const useCachedFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      setError(null);

      const cachedData = getFromCache(url);
      if (cachedData) {
        setData(cachedData);
        setLoading(false);
        return;
      }

      try {
        const response = await fetch(url);
        const data = await response.json();
        setData(data);
        setToCache(url, response.data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
};

export default useCachedFetch;
```

```jsx
// App.js
import React from "react";
import useCachedFetch from "./useCachedFetch";

const App = () => {
  const { data, loading, error } = useCachedFetch(
    "https://api.example.com/data"
  );

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h1>Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default App;
```

</details>

<!-- TOC --><a name="problem-4-debounced-search"></a>

### Problem 4: Debounced Search

- **Description:**
  - Create a search input box.
  - As the user types in the search box, fetch suggestions from an API but only after the user stops typing for 300 milliseconds.
  - Display the fetched suggestions in a list below the search box.

<details>
  <summary> Solution: </summary>

    - **1st Solution with out  debounce** util

        ```jsx
        import React, { useState, useEffect, useRef } from "react";

        const DebouncedSearch1 = () => {
          const [searchTerm, setSearchTerm] = useState("");
          const [suggestions, setSuggestions] = useState([]);
          const [isLoading, setIsLoading] = useState(false);
          const debounceTimeout = useRef(null);

          const fetchSuggestions = async (term) => {
            setIsLoading(true);
            try {
              const response = await fetch(
                `https://dummyjson.com/products/search?q=${term}`
              );
              if (response.ok) {
                const responseData = await response.json();
                setSuggestions(responseData.products);
              }
            } catch (error) {
              console.error("Error fetching suggestions:", error);
              setSuggestions([]);
            }
            setIsLoading(false);
          };

          useEffect(() => {
            if (debounceTimeout.current) {
              clearTimeout(debounceTimeout.current);
            }

            if (searchTerm) {
              debounceTimeout.current = setTimeout(() => {
                fetchSuggestions(searchTerm);
              }, 300);
            } else {
              setSuggestions([]);
            }

            return () => {
              clearTimeout(debounceTimeout.current);
            };
          }, [searchTerm]);

          const handleInputChange = (event) => {
            setSearchTerm(event.target.value);
          };

          return (
            <div>
              <input
                type="text"
                placeholder="Search..."
                value={searchTerm}
                onChange={handleInputChange}
              />
              {isLoading ? (
                <p>Loading...</p>
              ) : (
                <ul>
                  {suggestions.map((product) => (
                    <li key={product.id}>{product.title}</li>
                  ))}
                </ul>
              )}
            </div>
          );
        };

        export default DebouncedSearch1;

        ```

    - **2nd Solution with  debounce** util

        ```jsx
        import React, { useState, useCallback } from "react";

        const debounce = (func, delay) => {
          let timeout;
          return (...args) => {
            if (timeout) clearTimeout(timeout);
            timeout = setTimeout(() => {
              func(...args);
            }, delay);
          };
        };

        const DebouncedSearch = () => {
          const [searchTerm, setSearchTerm] = useState("");
          const [suggestions, setSuggestions] = useState([]);
          const [isLoading, setIsLoading] = useState(false);

          const fetchSuggestions = async (term) => {
            setIsLoading(true);
            try {
              const response = await fetch(
                `https://dummyjson.com/products/search?q=${term}`
              );
              if (response.ok) {
                const responseData = await response.json();
                setSuggestions(responseData.products);
              }
            } catch (error) {
              console.error("Error fetching suggestions:", error);
              setSuggestions([]);
            }
            setIsLoading(false);
          };

          const debouncedFetchSuggestions = useCallback(
            debounce(fetchSuggestions, 300),
            []
          );

          const handleInputChange = (event) => {
            const value = event.target.value;
            setSearchTerm(value);
            debouncedFetchSuggestions(value);
          };

          return (
            <div>
              <input
                type="text"
                placeholder="Search..."
                value={searchTerm}
                onChange={handleInputChange}
              />
              {isLoading ? (
                <p>Loading...</p>
              ) : (
                <ul>
                  {suggestions.map((product) => (
                    <li key={product.id}>{product.title}</li>
                  ))}
                </ul>
              )}
            </div>
          );
        };

        export default DebouncedSearch
        ```

    - 3rd Solution using custom hook useDebounce

        ```jsx
        import React, { useState, useEffect } from "react";

        const useDebouncedValue = (value, delay) => {
          const [debouncedValue, setDebouncedValue] = useState(value);

          useEffect(() => {
            let timer;
            timer = setTimeout(() => {
              setDebouncedValue(value);
            }, delay);

            return () => {
              clearTimeout(timer);
            };
          }, [value, delay]);

          return debouncedValue;
        };

        const DebouncedSearch3 = () => {
          const [searchTerm, setSearchTerm] = useState("");
          const [suggestions, setSuggestions] = useState([]);
          const [isLoading, setIsLoading] = useState(false);
          const debounceSearchTerm = useDebouncedValue(searchTerm, 300);

          const fetchSuggestions = async (term) => {
            setIsLoading(true);
            try {
              const response = await fetch(
                `https://dummyjson.com/products/search?q=${term}`
              );
              if (response.ok) {
                const responseData = await response.json();
                setSuggestions(responseData.products);
              }
            } catch (error) {
              console.error("Error fetching suggestions:", error);
              setSuggestions([]);
            } finally {
              setIsLoading(false);
            }
          };

          useEffect(() => {
            if (debounceSearchTerm) {
              fetchSuggestions(debounceSearchTerm);
            } else {
              setSuggestions([]);
            }
          }, [debounceSearchTerm]);

          const handleInputChange = (event) => {
            setSearchTerm(event.target.value);
          };

          return (
            <div>
              <input
                type="text"
                placeholder="Search..."
                value={searchTerm}
                onChange={handleInputChange}
              />
              {isLoading ? (
                <p>Loading...</p>
              ) : (
                <ul>
                  {suggestions.map((product) => (
                    <li key={product.id}>{product.title}</li>
                  ))}
                </ul>
              )}
            </div>
          );
        };

        export default DebouncedSearch3;

        ```

</details>

<!-- TOC --><a name="problem-41-debounce-and-reset-input-value"></a>

### Problem 4.1 : Debounce and Reset input value

- **Description:**
  - Create an input box and a paragraph tag under this input box.
  - As User enters in that input box. Once the user stops entering into the input box, the program should wait for 4 seconds,
  - After 4 seconds the entered value should appear in the paragraph tag and the input box should be cleared.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    import React, { useRef, useState } from "react";
    
    const DebounceInputAndReset1 = () => {
      const [inputValue, setInputValue] = useState("");
      const [displayValue, setDisplayValue] = useState("");
      const debounceTimeout = useRef(null);
    
      const handleChange = (e) => {
        const value = e.target.value;
        setInputValue(value);
    
        if (debounceTimeout.current) {
          clearTimeout(debounceTimeout.current);
        }
    
        debounceTimeout.current = setTimeout(() => {
          setDisplayValue(value);
          setInputValue("");
        }, 4000);
      };
    
      return (
        <div>
          <input
            type="text"
            value={inputValue}
            onChange={handleChange}
            placeholder="Type something..."
          />
          <p>{displayValue}</p>
        </div>
      );
    };
    
    export default DebounceInputAndReset1;
    
    ```
</details>

<!-- TOC --><a name="problem-5-form-validation"></a>

### Problem 5: Form Validation

- **Description:**
  - Create a simple form with inputs for a username and password.
  - The username should be at least 4 characters long, and the password should be at least 8 characters long.
  - Display validation messages for each input field as the user types.
  - Disable the submit button if either input is invalid.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    /* 
    
    - **Description:**
        - Create a simple form with inputs for a username and password.
        - The username should be at least 4 characters long, and the password should be at least 8 characters long.
        - Display validation messages for each input field as the user types.
        - Disable the submit button if either input is invalid. 
    */
    
    import { useEffect, useState } from "react";
    
    const initialFormData = { username: "", password: "" };
    const initialError = { username: false, password: false };
    
    const FormValidation = () => {
      const [formData, setFormData] = useState(initialFormData);
      const [error, setError] = useState(initialError);
    
      const handleChange = (e) => {
        setFormData((prevData) => ({
          ...prevData,
          [e.target.name]: e.target.value,
        }));
      };
    
      useEffect(() => {
        setError({
          username: formData.username.length > 0 && formData.username.length < 4,
          password: formData.password.length > 0 && formData.password.length < 8,
        });
      }, [formData]);
    
      return (
        <form autoComplete="off">
          <div>
            <input
              type="text"
              name="username"
              onChange={handleChange}
              value={formData.username}
              autoComplete="off"
            />
            {error.username && <p>Username should be at least 4 characters long</p>}
          </div>
          <br />
    
          <div>
            <input
              type="password"
              name="password"
              onChange={handleChange}
              value={formData.password}
              autoComplete="new-password"
            />
            {error.password && <p>password should be at least 8 characters long</p>}
          </div>
          <br />
    
          <input type="submit" disabled={error.password || error.username} />
        </form>
      );
    };
    
    export default FormValidation;
    
    ```
</details>

<!-- TOC --><a name="problem-61-countdown-timer"></a>

### Problem 6.1: Countdown Timer

- **Description:**
  - Create a countdown timer that starts from a given number of seconds.
  - Provide a button to start the countdown.
  - Display the remaining time in seconds in a paragraph tag.
  - When the timer reaches zero, display a message saying "Time's up!".

<details>
  <summary> Solution: </summary>

    - 1st Without useEffect Hook

        ```jsx
        import React, { useRef, useState } from "react";

        const CountdownTimer1 = () => {
          const [inputValue, setInputValue] = useState("");
          const [count, setCount] = useState(null);
          const interval = useRef(null);

          const startTimer = () => {
            setCount(+inputValue);

            interval.current = setInterval(() => {
              setCount((prev) => {
                if (prev === 0) {
                  clearInterval(interval.current);
                }
                return prev - 1;
              });
            }, 1000);
          };

          return (
            <div>
              <div>
                <input
                  type="text"
                  onChange={(e) => setInputValue(e.target.value)}
                  value={inputValue}
                />{" "}
                <button onClick={startTimer}>Start</button>
              </div>
              <p>{count === -1 ? "Time's up!" : count}</p>
              <br />
            </div>
          );
        };

        export default CountdownTimer1;

        ```

    - 3rd using useEffect Hook

        ```jsx
        import React, { useEffect, useRef, useState } from "react";

        const CountdownTimer3 = () => {
          const [inputValue, setInputValue] = useState("");
          const [count, setCount] = useState();
          const [isActive, setIsActive] = useState(false);
          const interval = useRef(null);

          useEffect(() => {
            if (isActive) {
              interval.current = setInterval(() => {
                setCount((prev) => {
                  if (prev === 0) {
                    setIsActive(false);
                  }
                  return prev - 1;
                });
              }, 1000);
            }

            return () => {
              interval.current && clearInterval(interval.current);
            };
          }, [isActive]);

          const startTimer = () => {
            setCount(+inputValue);
            setIsActive(true);
          };

          return (
            <div>
              <div>
                <input
                  type="text"
                  onChange={(e) => setInputValue(e.target.value)}
                  value={inputValue}
                />{" "}
                <button onClick={startTimer}>Start</button>
              </div>
              <p>{count === -1 ? "Time's up!" : count}</p>
              <br />
            </div>
          );
        };

        export default CountdownTimer3;

        ```

</details>

<!-- TOC --><a name="problem-62-countdown-timer-with-start-and-stop-button"></a>

### Problem 6.2: Countdown Timer with start and stop button

- **Description:**
  - Create a countdown timer that starts from a given number of seconds.
  - Provide a common button to start/stop the countdown.
  - Display the remaining time in seconds in a paragraph tag.
  - when clicked button to stop and again clicked to start timer it's should start from previous remaining seconds
  - When the timer reaches zero, display a message saying "Time's up!".

<details>
  <summary> Solution: </summary>

    - 1st Without useEffect Hook

        ```jsx
        /*
        ### Problem 6.2: Countdown Timer with start-stop

        - **Description:**
            - Create a countdown timer that starts from a given number of seconds.
            - Provide a common button to start/stop the countdown.
            - Display the remaining time in seconds in a paragraph tag.
            - when clicked button to stop and again clicked to start timer it's should start from previous remaining seconds
            - When the timer reaches zero, display a message saying "Time's up!".
        */

        import React, { useRef, useState } from "react";

        const CountdownTimer1 = () => {
          const [inputValue, setInputValue] = useState("");
          const [count, setCount] = useState(null);
          const [isActive, setIsActive] = useState(false);

          const interval = useRef(null);

          const timerHandler = (start) => {
            if (!start) {
              clearInterval(interval.current);
              setIsActive(false);
              return;
            }

            setCount((prevValue) => {
              if (prevValue > -1) {
                return prevValue;
              }

              return +inputValue;
            });
            setIsActive(true);

            interval.current = setInterval(() => {
              setCount((prev) => {
                if (prev === 0) {
                  clearInterval(interval.current);
                  setIsActive(false);
                }
                return prev - 1;
              });
            }, 1000);
          };

          return (
            <div>
              <div>
                <input
                  type="text"
                  onChange={(e) => setInputValue(e.target.value)}
                  value={inputValue}
                />{" "}
                <button onClick={() => timerHandler(!isActive)}>
                  {isActive ? "Stop" : "Start"}
                </button>
              </div>
              <p>{count === -1 ? "Time's up!" : count}</p>
              <br />
            </div>
          );
        };

        export default CountdownTimer1;

        ```

    - 2nd using  **useEffect**

        ```jsx
        /*
        ### Problem 6.2: Countdown Timer with start-stop

        - **Description:**
            - Create a countdown timer that starts from a given number of seconds.
            - Provide a common button to start/stop the countdown.
            - Display the remaining time in seconds in a paragraph tag.
            - when clicked button to stop and again clicked to start timer it's should start from previous remaining seconds
            - When the timer reaches zero, display a message saying "Time's up!".
        */

        import React, { useEffect, useRef, useState } from "react";

        const CountdownTimer3 = () => {
          const [inputValue, setInputValue] = useState("");
          const [count, setCount] = useState();
          const [isActive, setIsActive] = useState(false);
          const interval = useRef(null);

          useEffect(() => {
            if (isActive) {
              interval.current = setInterval(() => {
                setCount((prev) => {
                  if (prev === 0) {
                    setIsActive(false);
                  }
                  return prev - 1;
                });
              }, 1000);
            }

            return () => {
              interval.current && clearInterval(interval.current);
            };
          }, [isActive]);

          const startTimer = () => {
            setCount((prevValue) => {
              if (prevValue > -1) {
                return prevValue;
              }

              return +inputValue;
            });
            setIsActive(!isActive);
          };

          return (
            <div>
              <div>
                <input
                  type="text"
                  onChange={(e) => setInputValue(e.target.value)}
                  value={inputValue}
                />{" "}
                <button onClick={startTimer}>{isActive ? "Stop" : "Start"}</button>
              </div>
              <p>{count === -1 ? "Time's up!" : count}</p>
              <br />
            </div>
          );
        };

        export default CountdownTimer3;

        ```

</details>

<!-- TOC --><a name="problem-7-dynamic-list"></a>

### Problem 7: Dynamic List

**Description:**

- Create an input box and a button.
- When the user types a value in the input box and clicks the button, add the value to a list displayed below.
- Each item in the list should have a delete button that removes it from the list.
<details>
  <summary> Solution: </summary>
    
    ```jsx
    import React, { useState } from "react";
    
    const DynamicList = () => {
      const [inputValue, setInputValue] = useState("");
      const [list, setList] = useState([]);
    
      const addItem = () => {
        setList([...list, inputValue]);
        setInputValue("");
      };
    
      const deleteItem = (itemToBeRemoved) => {
        const updatedList = list.filter((item) => itemToBeRemoved !== item);
        setList(updatedList);
      };
    
      const randomString = () => Math.random().toString(36).slice(2);
    
      return (
        <div>
          <div style={{ marginBottom: "1rem" }}>
            <input
              type="text"
              onChange={(e) => setInputValue(e.target.value)}
              value={inputValue}
            />{" "}
            <button onClick={addItem}>Add</button>
          </div>
    
          <div>
            {list.map((item, i) => (
              <div key={i + randomString()} style={{ marginBottom: "1rem" }}>
                <span>{item} </span>
                <button onClick={() => deleteItem(item)}>Delete</button>
              </div>
            ))}
          </div>
        </div>
      );
    };
    
    export default DynamicList;
    
    ```
</details>

<!-- TOC --><a name="problem-8-controlled-tabs"></a>

### Problem 8: Controlled Tabs

**Description:**

- Create a tab component with at least three tabs.
- Each tab should display different content when clicked.
- Maintain the active tab's state and ensure the correct content is displayed.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    /* 
    **Description:**
    
    - Create a tab component with at least three tabs.
    - Each tab should display different content when clicked.
    - Maintain the active tab's state and ensure the correct content is displayed. 
    
    // states : activeTab , tabContent
    // props : { tabs  }
    
    */
    
    import { useEffect, useState } from "react";
    
    const defaultTabs = [
      {
        id: 1,
        name: "Tab 1",
        content: (
          <>
            <h1>Tab 1 </h1>
            <p>
              Tab 1 Content Lorem ipsum dolor sit amet consectetur, adipisicing
              elit. Nobis excepturi repellendus repudiandae, aliquam reiciendis
              provident voluptatibus odio accusamus officia totam esse illum soluta
              facilis at! Id minus rem eligendi inventore.
            </p>
          </>
        ),
      },
      {
        id: 2,
        name: "Tab 2",
        content: (
          <>
            <h1>Tab 2 </h1>
            <p>
              Tab 2 Content Lorem ipsum dolor sit amet consectetur, adipisicing
              elit. Nobis excepturi repellendus repudiandae, aliquam reiciendis
              provident voluptatibus odio accusamus officia totam esse illum soluta
              facilis at! Id minus rem eligendi inventore.
            </p>
          </>
        ),
      },
      {
        id: 3,
        name: "Tab 3",
        content: (
          <>
            <h1>Tab 3 </h1>
            <p>
              Tab 3 Content Lorem ipsum dolor sit amet consectetur, adipisicing
              elit. Nobis excepturi repellendus repudiandae, aliquam reiciendis
              provident voluptatibus odio accusamus officia totam esse illum soluta
              facilis at! Id minus rem eligendi inventore.
            </p>
          </>
        ),
      },
    ];
    
    const Tabs = ({ tabs = defaultTabs }) => {
      const [activeTab, setActiveTab] = useState(+tabs[0].id);
      const [tabContent, setTabContent] = useState(tabs[0].content);
    
      useEffect(() => {
        const content = tabs.find((tab) => tab.id === activeTab).content;
        setTabContent(content);
      }, [activeTab, tabs]);
    
      return (
        <div>
          <div
            style={{
              display: "flex",
              gap: "1rem",
              justifyContent: "center",
              marginBottom: "1rem",
            }}
          >
            {tabs.map((tab, id) => (
              <button
                key={id}
                onClick={() => setActiveTab(+tab.id)}
                style={{
                  backgroundColor: `${activeTab === tab.id ? "black" : "white"}`,
                  color: `${activeTab === tab.id ? "white" : "black"}`,
                }}
              >
                {tab.name}
              </button>
            ))}
          </div>
          <div>{tabContent}</div>
        </div>
      );
    };
    
    export default Tabs;
    
    ```
</details>

<!-- TOC --><a name="problem-82-dynamic-routes-tabs-navigations"></a>

### Problem 8.2: Dynamic Routes Tabs Navigations

**Description:**

- Create a tab component with at least three tabs.
- when clicked on any tab it should navigate to a different route and it's content should be displayed
- Maintain the active tab's state and ensure the correct content is displayed.

<details>
  <summary> Solution: </summary>

```jsx
/* 
**Description:**

- Create a tab component with at least three tabs.
- when clicked on any tab it should navigate to a different route and it's content should be displayed
- Maintain the active tab's state and ensure the correct content is displayed. 

LLD 
// router : consist all routes
// component : Layout , TabBar , TabContent

TabBar
// props : { tabs  }

TabContent:
// states : content
// params : tabName
// useEffect : fetch and set content 

*/

import { createBrowserRouter, RouterProvider } from "react-router-dom";
import TabContent from "./components/TabContent";
import Layout from "./components/Layout";

const router = createBrowserRouter([
  {
    element: <Layout />,
    path: "/",
    errorElement: "Error sorry!",
    children: [
      {
        path: "/:tabName",
        element: <TabContent />,
      },
    ],
  },
]);

const DynamicRoutesNavigation = () => {
  return <RouterProvider router={router} />;
};

export default DynamicRoutesNavigation;
```

**Layout.jsx**

```jsx
import React from "react";
import { Outlet } from "react-router-dom";
import Tabs from "./Tabs";

const Layout = () => {
  return (
    <div>
      <Tabs />
      <Outlet />
    </div>
  );
};

export default Layout;
```

**TabContent.jsx**

```
import { useEffect, useState } from "react";
import { useParams } from "react-router-dom";
import { defaultTabs } from "../Data";

const TabContent = () => {
  const [content, setContent] = useState();
  const { tabName } = useParams();
  useEffect(() => {
    const contentData = defaultTabs.find(
      (tab) => tab.name.replace(" ", "-").toLocaleLowerCase() === tabName
    ).content;
    setContent(contentData);
  }, [tabName]);

  return content;
};

export default TabContent;
```

**Tabs.jsx**

```jsx
import React from "react";
import { NavLink } from "react-router-dom";
import { defaultTabs } from "../Data";

const Tabs = ({ tabs = defaultTabs }) => {
  return (
    <div>
      <div
        style={{
          display: "flex",
          gap: "1rem",
          justifyContent: "center",
          marginBottom: "1rem",
        }}
      >
        {tabs.map((tab, id) => (
          <NavLink
            key={id}
            to={`/${tab.name.replace(" ", "-").toLocaleLowerCase()}`}
            style={({ isActive }) => ({
              backgroundColor: `${isActive ? "black" : "white"}`,
              color: `${isActive ? "white" : "black"}`,
            })}
          >
            {tab.name}
          </NavLink>
        ))}
      </div>
    </div>
  );
};

export default Tabs;
```

</details>

<!-- TOC --><a name="problem-9-lightdark-mode-toggle"></a>

### Problem 9: Light/Dark Mode Toggle

**Description:**

- Create a toggle switch to switch between light and dark modes.
- Apply different styles for light and dark modes to the entire application.
- Persist the selected mode in local storage so that it is maintained across page reloads.

    <details>
      <summary> Solution: </summary>

        - **Solution 1 - SwitchTheme component**

            Home.jsx

            ```jsx
            import React from "react";
            import SwitchTheme from "./SwitchTheme";
            import "./styles.css";

            const Home = () => {
              return (
                <div>
                  <header
                    style={{
                      textAlign: "center",
                      display: "flex",
                      justifyContent: "space-around",
                    }}
                  >
                    {" "}
                    <h1> Header </h1>
                    <SwitchTheme />
                  </header>
                  <main>
                    <h1>Heading Light Dark</h1>
                    <p>
                      Lorem ipsum, dolor sit amet consectetur adipisicing elit. Doloribus
                      praesentium perspiciatis, esse, quis voluptatum repellendus, vitae
                      expedita hic optio deserunt tempora aspernatur corporis iusto libero
                      nam quisquam minus. Quo, eveniet?
                    </p>
                  </main>
                  <footer style={{ textAlign: "center" }}>
                    {" "}
                    <h1> Footer </h1>{" "}
                  </footer>
                </div>
              );
            };

            export default Home;
            ```

            **SwitchTheme.jsx**

            ```jsx
            /*
            ### Problem 9: Light/Dark Mode Toggle

            **Description:**

            - Create a toggle switch to switch between light and dark modes.
            - Apply different styles for light and dark modes to the entire application.
            - Persist the selected mode in local storage so that it is maintained across page reloads.

             */

            import React, { useEffect, useState } from "react";

            const SwitchTheme = () => {
              const [isDarkMode, setIsDarkMode] = useState(
                JSON.parse(localStorage.getItem("isDarkMode")) || false
              );

              useEffect(() => {
                const theme = isDarkMode ? "dark" : "light";
                document.documentElement.setAttribute("data-theme", theme);
                localStorage.setItem("isDarkMode", isDarkMode);
              }, [isDarkMode]);

              return (
                <button
                  onClick={() => {
                    setIsDarkMode(!isDarkMode);
                  }}
                  style={{
                    color: isDarkMode ? "black" : "white",
                    backgroundColor: isDarkMode ? "white" : "black",
                    width: "50px",
                    height: "50px",
                    borderRadius: "10px",
                  }}
                >
                  {isDarkMode ? "Light" : "Dark"}
                </button>
              );
            };

            export default SwitchTheme;
            ```

            **styles.css**

            ```css
            body {
              font-family: Arial, sans-serif;
              margin: 0;
              padding: 0 30px;
            }

            [data-theme="light"] {
              --bg-color: #ffffff;
              --text-color: #333333;
              --heading-color: #444444;
            }

            /* Dark mode styles */
            [data-theme="dark"] {
              --bg-color: #333333;
              --text-color: #ffffff;
              --heading-color: #dddddd;
            }

            body {
              background-color: var(--bg-color);
              color: var(--text-color);
            }

            h1 {
              color: var(--heading-color);
            }

            ```

        - **Solution 2 - using custom Hook useTheme**

            ```jsx
            import React from "react";
            import "./styles.css";
            import useTheme from "./useTheme";

            const ToggleTheme2 = () => {
              const { isDarkMode, toggleTheme, theme } = useTheme();

              return (
                <div>
                  <header
                    style={{
                      textAlign: "center",
                      display: "flex",
                      justifyContent: "space-around",
                    }}
                  >
                    {" "}
                    <h1> Header </h1>
                    <button
                      onClick={toggleTheme}
                      style={{
                        color: isDarkMode ? "black" : "white",
                        backgroundColor: isDarkMode ? "white" : "black",
                        width: "50px",
                        height: "50px",
                        borderRadius: "10px",
                      }}
                    >
                      {isDarkMode ? "Light" : "Dark"}
                    </button>
                  </header>
                  <main>
                    <h1>Heading {theme} mode</h1>
                    <p>
                      Using custom hook Lorem ipsum, dolor sit amet consectetur adipisicing
                      elit. Doloribus praesentium perspiciatis, esse, quis voluptatum
                      repellendus, vitae expedita hic optio deserunt tempora aspernatur
                      corporis iusto libero nam quisquam minus. Quo, eveniet?
                    </p>
                  </main>
                  <footer style={{ textAlign: "center" }}>
                    {" "}
                    <h1> Footer </h1>{" "}
                  </footer>
                </div>
              );
            };

            export default ToggleTheme2;

            ```

            **useTheme.jsx**

            ```jsx
            /*

            ### Problem 9: Light/Dark Mode Toggle

            **Description:**

            - Create a toggle switch to switch between light and dark modes.
            - Apply different styles for light and dark modes to the entire application.
            - Persist the selected mode in local storage so that it is maintained across page reloads.

             */

            import { useEffect, useState } from "react";

            const useTheme = (isDark = false) => {
              const [isDarkMode, setIsDarkMode] = useState(
                JSON.parse(localStorage.getItem("isDarkMode")) || isDark
              );
              const theme = isDarkMode ? "dark" : "light";

              useEffect(() => {
                document.documentElement.setAttribute("data-theme", theme);
                localStorage.setItem("isDarkMode", isDarkMode);
              }, [isDarkMode]);

              const toggleTheme = () => setIsDarkMode(!isDarkMode);

              return { theme, toggleTheme, isDarkMode, setIsDarkMode };
            };

            export default useTheme;

            ```

            **styles.css**

            ```css
            body {
              font-family: Arial, sans-serif;
              margin: 0;
              padding: 0 30px;
            }

            [data-theme="light"] {
              --bg-color: #ffffff;
              --text-color: #333333;
              --heading-color: #444444;
            }

            /* Dark mode styles */
            [data-theme="dark"] {
              --bg-color: #333333;
              --text-color: #ffffff;
              --heading-color: #dddddd;
            }

            body {
              background-color: var(--bg-color);
              color: var(--text-color);
            }

            h1 {
              color: var(--heading-color);
            }

            ```

  </details>

<!-- TOC --><a name="problem-10-modal-popup"></a>

### Problem 10: Modal Popup

**Description:**

- Create a button that opens a modal popup when clicked.
- The modal should have a close button to hide it.
- Ensure that clicking outside the modal or pressing the "Escape" key also closes the modal.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    import { useEffect } from "react";
    import "./styles.css";
    
    export const Modal = ({
      title = "Modal Header",
      content,
      cancelBtnLabel = "Cancel",
      confirmBtnLabel = "Confirm",
      onClose,
      onConfirm,
    }) => {
      const handleOverlayClick = (e) => {
        if (e.target === e.currentTarget || e.key === "Escape") {
          onClose();
        }
      };
    
      useEffect(() => {
        document.addEventListener("keydown", handleOverlayClick);
    
        return () => {
          document.removeEventListener("keydown", handleOverlayClick);
        };
      }, []);
    
      return (
        <div className="modal" onClick={handleOverlayClick}>
          <div className="modal-content">
            <header>
              <h3>{title}</h3> <button onClick={onClose}>❌</button>
            </header>
            <main>{content}</main>
            <footer>
              <button onClick={onClose}>{cancelBtnLabel}</button>
              <button onClick={onConfirm}>{confirmBtnLabel}</button>
            </footer>
          </div>
        </div>
      );
    };
    ```
    
    ```jsx
    /* 
    ### Problem 10: Modal Popup
    
    **Description:**
    
    - Create a button that opens a modal popup when clicked.
    - The modal should have a close button to hide it.
    - Ensure that clicking outside the modal or pressing the "Escape" key also closes the modal.
    
     */
    import React, { useState } from "react";
    import { Modal } from "./Modal";
    
    const ModalPopup = () => {
      const [isModalOpen, setIsModalOpen] = useState();
    
      const toggleModal = () => {
        setIsModalOpen(!isModalOpen);
      };
    
      const modalContent = (
        <>
          <h2>Modal Popup</h2>
          <p>This is a modal popup. Click outside or press "Escape" to close it.</p>
        </>
      );
    
      return (
        <div>
          <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Neque aperiam
            laboriosam explicabo debitis voluptas reiciendis ullam ratione magnam,
            obcaecati vitae natus numquam sapiente tempora assumenda magni
            repellendus expedita nulla a!
          </p>
          <button onClick={toggleModal}>open Modal</button>
          {isModalOpen && <Modal onClose={toggleModal} content={modalContent} />}
        </div>
      );
    };
    
    export default ModalPopup;
    ```
    
    **styles.css**
    
    ```
    .modal {
      position: fixed;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    
      .modal-content {
        width: 600px;
        max-width: 1200px;
        min-height: 600px;
        margin: auto;
        background-color: white;
        box-shadow: 0 0 20px 0px #00000073;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        padding: 1rem;
        border-radius: 5px;
        position: relative;
        z-index: 1001;
    
        header {
          display: flex;
          justify-content: space-between;
        }
        main {
          min-height: 80%;
          width: 100%;
        }
        footer {
          display: flex;
          gap: 1rem;
          justify-content: center;
        }
      }
    }
    ```
</details>

<!-- TOC --><a name="problem-11-infinite-scroll"></a>

### Problem 11: Infinite Scroll

**Description:**

- Create a component that fetches and displays data in an infinite scroll manner.
- As the user scrolls to the bottom of the list, fetch more data and append it to the existing list.
- Display a loading indicator while fetching the new data.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    import React, { memo, useEffect, useRef, useState } from "react";
    const LIMIT = 10;
    
    const debounce = (func, delay) => {
      let timeout;
      return (...args) => {
        if (timeout) clearTimeout(timeout);
        timeout = setTimeout(() => {
          func(...args);
        }, delay);
      };
    };
    
    const FetchDataOnScroll = () => {
      const [products, setProducts] = useState([]);
      const [loading, setLoading] = useState(false);
      const [error, setError] = useState(null);
      const [page, setPage] = useState(0);
    
      const loaderRef = useRef(null);
      const listContainerRef = useRef(null);
    
      useEffect(() => {
        /* Why useEffect running twice and how to handle it well in React? */
        // https://react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development
        // https://stackoverflow.com/questions/72238175/why-useeffect-running-twice-and-how-to-handle-it-well-in-react
        // https://medium.com/@arm.ninoyan/fixed-react-18-useeffect-runs-twice-8480f0bd837f
    
        const abortController = new AbortController();
    
        const fetchData = async () => {
          setError("");
          setLoading(true);
          try {
            const response = await fetch(
              `https://dummyjson.com/products?limit=${LIMIT}&skip=${page * LIMIT}`,
              {
                signal: abortController.signal,
              }
            );
            if (response.ok) {
              const result = await response.json();
    
              setProducts((prevData) => [...prevData, ...result.products]);
            } else {
              throw new Error("Failed to fetch data");
            }
          } catch (error) {
            setError(error.message || error.body.message || "Error");
          } finally {
            setLoading(false);
          }
        };
        fetchData();
    
        return () => {
          abortController.abort();
        };
      }, [page]);
    
      const handleObserver = debounce((entries) => {
        const target = entries[0];
        if (target.isIntersecting) {
          setPage((prevPage) => prevPage + 1);
        }
      }, 500);
    
      useEffect(() => {
        const option = {
          root: listContainerRef.current,
          rootMargin: "10px",
          threshold: 1.0,
        };
        if (products.length > 0) {
          const observer = new IntersectionObserver(handleObserver, option);
          if (loaderRef.current) observer.observe(loaderRef.current);
          return () => {
            if (loaderRef.current) observer.unobserve(loaderRef.current);
          };
        }
      }, [products]);
    
      return (
        <div
          style={{
            maxHeight: "400px",
            overflowY: "scroll",
            border: "1px solid black",
            borderRadius: "5px",
          }}
          ref={listContainerRef}
        >
          {products.map(({ id, title, description }) => (
            <div key={id}>
              <h3>{title}</h3>
              <p>{description}</p>
            </div>
          ))}
          {loading && <p>Loading...</p>}
          {error && <p>{error}</p>}
    
          <div ref={loaderRef} />
        </div>
      );
    };
    
    export default memo(FetchDataOnScroll);
    
    ```
</details>

<!-- TOC --><a name="problem-12-sorting-a-list"></a>

### Problem 12: Sorting a List

**Description:**

- Create a list of items with a sort button.
- Clicking the sort button should toggle between sorting the list in ascending and descending order.
- Display the sorted list accordingly.

<details>
  <summary> Solution: </summary>

    - **Sort by price only**

        ```jsx
        import { useEffect, useState } from "react";

        const SortList1 = () => {
          const [products, setProducts] = useState([]);
          const [sortOrder, setSortOrder] = useState("des");
          const isAscOrder = sortOrder === "asc";

          useEffect(() => {
            const fetchProducts = async () => {
              try {
                const response = await fetch(`https://dummyjson.com/products`);
                const data = await response.json();
                setProducts(data.products);
              } catch (error) {
                console.log(error);
              }
            };

            fetchProducts();
          }, []);

          useEffect(() => {
            // Create a new array for sorting
            const sortedProducts = [...products].sort((a, b) =>
              isAscOrder ? a.price - b.price : b.price - a.price
            );
            setProducts(sortedProducts);
          }, [sortOrder]);

          const toggleSortOder = () =>
            setSortOrder((prevOrder) => (prevOrder === "asc" ? "des" : "asc"));

          return (
            <div
              style={{
                maxHeight: "400px",
                overflowY: "scroll",
                border: "1px solid black",
                borderRadius: "5px",
                padding: "1rem",
              }}
            >
              {products.length > 0 && (
                <button
                  onClick={toggleSortOder}
                  style={{
                    border: "1px solid black",
                    borderRadius: "5px",
                    padding: "0.5rem",
                    marginBottom: "1rem",
                  }}
                >
                  Sort {isAscOrder ? "Descending" : "Ascending"}
                </button>
              )}
              {products?.map(({ id, title, description, price }) => (
                <div key={id}>
                  <h3>{title}</h3>
                  <span>{price}</span>
                  <p>{description}</p>
                </div>
              ))}
            </div>
          );
        };

        export default SortList1;

        ```

    - **Reusable customized Component**

        ```jsx
        import { useEffect, useState } from "react";
        const stringSort = {
          asc: function (a, b) {
            if (a < b) {
              return -1;
            }
            if (a > b) {
              return 1;
            }
            return 0;
          },
          des: function (a, b) {
            if (a < b) {
              return 1;
            }
            if (a > b) {
              return -1;
            }
            return 0;
          },
        };
        const numberSort = { asc: (a, b) => a - b, des: (a, b) => b - a };

        const SortList2 = ({ sortBy = "title" }) => {
          const [products, setProducts] = useState([]);
          const [sortOrder, setSortOrder] = useState("des");
          const isAscOrder = sortOrder === "asc";

          useEffect(() => {
            const fetchProducts = async () => {
              try {
                // 'https://dummyjson.com/products?sortBy=title&order=asc'
                const response = await fetch(`https://dummyjson.com/products`);
                const data = await response.json();
                setProducts(data.products);
              } catch (error) {
                console.log(error);
              }
            };

            fetchProducts();
          }, []);

          const compareFunc = {
            title: stringSort,
            price: numberSort,
            description: stringSort,
          };

          useEffect(() => {
            // Create a new array for sorting
            const sortedProducts = [...products].sort((a, b) =>
              compareFunc[sortBy][sortOrder](a[sortBy], b[sortBy])
            );
            setProducts(sortedProducts);
          }, [sortOrder]);

          const toggleSortOder = () =>
            setSortOrder((prevOrder) => (prevOrder === "asc" ? "des" : "asc"));

          return (
            <div
              style={{
                maxHeight: "400px",
                overflowY: "scroll",
                border: "1px solid black",
                borderRadius: "5px",
                padding: "1rem",
              }}
            >
              {products.length > 0 && (
                <button
                  onClick={toggleSortOder}
                  style={{
                    border: "1px solid black",
                    borderRadius: "5px",
                    padding: "0.5rem",
                    marginBottom: "1rem",
                  }}
                >
                  Sort {isAscOrder ? "Descending" : "Ascending"}
                </button>
              )}
              {products?.map(({ id, title, description, price }) => (
                <div key={id}>
                  <h3>{title}</h3>
                  <span>{price}</span>
                  <p>{description}</p>
                </div>
              ))}
            </div>
          );
        };

        export default SortList2;

        ```

    - Sort using BE API query params

        ```jsx
        import { useEffect, useState } from "react";

        const SortList2 = ({ sortBy = "price" }) => {
          const [products, setProducts] = useState([]);
          const [sortOrder, setSortOrder] = useState();
          const isAscOrder = sortOrder === "asc";

          useEffect(() => {
            const fetchProducts = async () => {
              try {
                const response = await fetch(
                  `https://dummyjson.com/products${
                    sortOrder ? `?sortBy=${sortBy}&order=${sortOrder}` : ""
                  }`
                );
                const data = await response.json();
                setProducts(data.products);
              } catch (error) {
                console.log(error);
              }
            };

            fetchProducts();
          }, [sortOrder, sortBy]);

          const toggleSortOder = () =>
            setSortOrder((prevOrder) => (prevOrder === "asc" ? "desc" : "asc"));

          return (
            <div
              style={{
                maxHeight: "400px",
                overflowY: "scroll",
                border: "1px solid black",
                borderRadius: "5px",
                padding: "1rem",
              }}
            >
              {products.length > 0 && (
                <button
                  onClick={toggleSortOder}
                  style={{
                    border: "1px solid black",
                    borderRadius: "5px",
                    padding: "0.5rem",
                    marginBottom: "1rem",
                  }}
                >
                  Sort {isAscOrder ? "Descending" : "Ascending"}
                </button>
              )}
              {products?.map(({ id, title, description, price }) => (
                <div key={id}>
                  <h3>{title}</h3>
                  <span>{price}</span>
                  <p>{description}</p>
                </div>
              ))}
            </div>
          );
        };

        export default SortList2;

        ```

</details>

<!-- TOC --><a name="problem-13-drag-and-drop"></a>

### Problem 13: Drag and Drop

**Description:**

- Implement a simple drag-and-drop interface where items from a list can be reordered by dragging.
- Use a library like `react-dnd` or the HTML5 drag-and-drop API to achieve this functionality.
<details>
  <summary> Solution: </summary>
    
    ```jsx
    import { useState } from "react";
    
    const initialItems = ["Item 1", "Item 2", "Item 3", "Item 4"];
    
    const DragAndDrop = () => {
      const [items, setItems] = useState(initialItems);
    
      const onDragStart = (e, index) => {
        e.dataTransfer.setData("dragItemIndex", index);
      };
    
      const onDragOver = (e) => {
        e.preventDefault();
      };
    
      const onDrop = (e, dropIndex) => {
        const dragItemIndex = e.dataTransfer.getData("dragItemIndex");
        const newItems = [...items];
        const draggedItem = newItems.splice(dragItemIndex, 1)[0];
        newItems.splice(dropIndex, 0, draggedItem);
        setItems(newItems);
      };
    
      return (
        <div
          style={{
            maxWidth: "200px",
            margin: "0 auto",
            border: "1px solid black",
            borderRadius: "5px",
            padding: "1rem",
          }}
        >
          <h3>Drag and Drop List</h3>
          <ul style={{ listStyleType: "none", padding: 0 }}>
            {items.map((item, index) => (
              <li
                key={index}
                draggable
                onDragStart={(e) => onDragStart(e, index)}
                onDragOver={onDragOver}
                onDrop={(e) => onDrop(e, index)}
                style={{
                  margin: "0.5rem 0",
                  padding: "0.5rem",
                  border: "1px solid #ccc",
                  borderRadius: "3px",
                  backgroundColor: "#f9f9f9",
                  cursor: "move",
                }}
              >
                {item}
              </li>
            ))}
          </ul>
        </div>
      );
    };
    
    export default DragAndDrop;
    
    ```
</details>

<!-- TOC --><a name="problem-141-accordion"></a>

### Problem 14.1: Accordion

- **Description:**
  - Create an accordion component with multiple sections.
  - Each section should have a title and content.
  - Clicking a section title should expand/collapse the section to show/hide the content.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    // - Create an accordion component with multiple sections.
    // - Each section should have a title and content.
    // - Clicking a section title should expand/collapse the section to show/hide the content.
    
    import React from "react";
    
    const defaultSections = [
      {
        id: 1,
        title: "Introduction",
        content:
          "This is the introduction section. It provides an overview of the topic.",
      },
      {
        id: 2,
        title: "Background",
        content: "This section provides background information and context.",
      },
      {
        id: 3,
        title: "Main Content",
        content:
          "Here is the main content of the topic, covering all the essential details.",
      },
      {
        id: 4,
        title: "Analysis",
        content:
          "This section contains an analysis of the main content, offering insights and perspectives.",
      },
      {
        id: 5,
        title: "Conclusion",
        content:
          "The conclusion summarizes the key points and provides final thoughts.",
      },
    ];
    // - Note : using details summary section will not close automaticlly.
    const Accordion1 = ({ sections = defaultSections }) => {
      return (
        <div className="accordion">
          {sections.map(({ id, title, content }) => (
            <div key={id}>
              <details>
                <summary style={{ cursor: "pointer" }}>
                  <h2 style={{ display: "inline-block" }}>{title}</h2>
                </summary>
                <p>{content}</p>
              </details>
            </div>
          ))}
        </div>
      );
    };
    
    export default Accordion1;
    
    ```
</details>

<!-- TOC --><a name="problem-142-accordion"></a>

### Problem 14.2: Accordion

- **Description:**
  - Create an accordion component with multiple sections.
  - Each section should have a title and content.
  - Clicking a section title should expand/collapse the section to show/hide the content.
  - Only one section should be expanded at a time.

<details>
  <summary> Solution: </summary>
</details>

<!-- TOC --><a name="problem-15-editable-table"></a>

### Problem 15: Editable Table

- **Description:** - Create a table with rows and columns. - Each cell in the table should be editable. - Provide a save button to save the edited data. - Validate the input data before saving and display validation messages if needed.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    /* 
    
    ### Problem 15: Editable Table
    
    **Description:**
    
    - Create a table with rows and columns.
    - Each cell in the table should be editable.
    - Provide a save button to save the edited data.
    - Validate the input data before saving and display validation messages if needed.
    
     */
    
    import React, { useState } from "react";
    const placeholderData = [
      { id: 1, name: "John Doe", age: 25, email: "john@example.com" },
      { id: 2, name: "Jane Smith", age: 30, email: "jane@example.com" },
      { id: 3, name: "Mike Johnson", age: 28, email: "mike@example.com" },
    ];
    
    const inputType = {
      id: "number",
      name: "text",
      age: "number",
      email: "email",
    };
    
    const EditableTable = ({ data = placeholderData }) => {
      const [tableData, setTableData] = useState(data);
      const [editingRowIndex, setEditingRowIndex] = useState(null);
      const [message, setMessage] = useState({});
    
      const updateTableData = (e, rowIndex, colName) => {
        const newData = [...tableData];
        newData[rowIndex][colName] = e.target.value;
    
        setTableData(newData);
        setEditingRowIndex(rowIndex);
      };
    
      const isInvalidData = () => {
        const { name, age, email } = tableData[editingRowIndex];
        let newErrors = {};
    
        newErrors = {
          name: !name && "Name is required",
          age: !age && "Age is required",
          email: !email && "Email is required",
        };
    
        if (!/^[\w-.]+@([\w-]+\.)+[\w-]{2,4}$/.test(email)) {
          newErrors.email = "Valid email is required.";
        }
    
        if (isNaN(age) || age <= 0) {
          newErrors.age = "Valid age is required.";
        }
    
        setMessage(newErrors);
        if (newErrors.name || newErrors.age || newErrors.email) {
          return true;
        }
    
        return false;
      };
    
      const handleSaveData = () => {
        if (!isInvalidData()) {
          setMessage({ success: "Data saved successfully" });
        }
      };
    
      return (
        <div style={{ width: "max-content", margin: "2rem auto" }}>
          <table border="2">
            <thead>
              <tr>
                {Object.keys(tableData[0]).map((key) => (
                  <th>{key.charAt(0).toUpperCase() + key.slice(1)}</th>
                ))}
                <th>Action</th>
              </tr>
            </thead>
            <tbody>
              {tableData.map((rowData, rowIndex) => (
                <tr>
                  {Object.entries(rowData).map(([colName, cellData]) => (
                    <td>
                      {colName === "id" ? (
                        cellData
                      ) : (
                        <input
                          type={inputType[colName]}
                          value={cellData}
                          onChange={(e) => updateTableData(e, rowIndex, colName)}
                          style={{
                            border:
                              editingRowIndex === rowIndex &&
                              message[colName] &&
                              message[colName] !== "success"
                                ? "2px solid red"
                                : "none",
                            outline: "none",
                          }}
                        />
                      )}
                    </td>
                  ))}
                  <td>
                    <button onClick={handleSaveData}>Save</button>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
          <div
            style={{ display: "flex", justifyContent: "center", margin: "1rem" }}
          >
            {message.name && <span style={{ color: "red" }}>{message.name}</span>}
            {message.age && <span style={{ color: "red" }}>{message.age}</span>}
            {message.email && <span style={{ color: "red" }}>{message.email}</span>}
            {message.success && (
              <span style={{ color: "green" }}>{message.success}</span>
            )}
          </div>
        </div>
      );
    };
    
    export default EditableTable;
    
    ```
</details>

<!-- TOC --><a name="problem-16-star-rating"></a>

### Problem 16: Star Rating

- **Description:**
  - Create a star rating component.
  - The user should be able to click on the stars to set the rating.
  - Display the current rating as highlighted stars.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    /* 
    ### Problem 16: Star Rating
    
    **Description:**
    
    - Create a star rating component.
    - The user should be able to click on the stars to set the rating.
    - Display the current rating as highlighted stars.
    
     */
    
    import { useState } from "react";
    
    import "./styles.css";
    
    const startIcon = <>&#9733;</>;
    
    function StarRating({ onChange, maxRating = 5 }) {
      const [rating, setRating] = useState(-1);
      const [ratingHovered, setRatingHovered] = useState(-1);
      const startIcons = Array(maxRating).fill(0);
    
      const ratingHandler = (i) => {
        setRating(i);
        onChange?.(i);
      };
    
      return (
        <div style={{ display: "flex", justifyContent: "center", gap: "1rem" }}>
          {startIcons.map((_, i) => {
            return (
              <button
                className={`rating-icon ${i <= rating ? "rated" : ""} ${
                  i <= ratingHovered ? "hovered" : ""
                }`}
                key={i}
                onClick={() => ratingHandler(i)}
                onMouseEnter={() => setRatingHovered(i)}
                onMouseLeave={() => setRatingHovered(-1)}
              >
                {startIcon}
              </button>
            );
          })}
        </div>
      );
    }
    
    export default StarRating;
    ```
    
    ```css
    .rating-icon {
      font-size: 3rem;
      border: none;
      background-color: white;
      cursor: pointer;
      &.hovered {
        color: gold;
      }
      &.rated {
        color: orange;
      }
    }
    ```
</details>

<!-- TOC --><a name="problem-17-weather-app"></a>

### Problem 17: Weather App

- **Description:** - Create a weather app that fetches weather data from an API based on the user’s input city. - Display the current weather and a 5-day forecast. - Show a loading indicator while fetching data and handle any potential errors gracefully.
<details>
  <summary> Solution: </summary>
    
    ```jsx
    import React, { useState } from "react";
    
    const WeatherApp = () => {
      const [city, setCity] = useState("");
      const [weatherData, setWeatherData] = useState(null);
      const [loading, setLoading] = useState(false);
      const [error, setError] = useState("");
    
      const apiKey = "4b307c3ffaab961d4c6b5083241b1423"; // Replace with your OpenWeatherMap API key
    
      const fetchWeatherData = async (city) => {
        setLoading(true);
        setError("");
        try {
          const response = await fetch(
            `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`
          );
    
          if (!response.ok) {
            throw new Error("City not found or API error. Please try again.");
          }
    
          const data = await response.json();
          setWeatherData(data);
        } catch (err) {
          setError(err.message);
        } finally {
          setLoading(false);
        }
      };
    
      const handleSearch = (e) => {
        e.preventDefault();
        if (city) {
          fetchWeatherData(city);
        } else {
          setError("Please enter a city.");
        }
      };
    
      return (
        <div style={{ padding: "20px", textAlign: "center" }}>
          <h1>5 day weather forecast</h1>
          <form onSubmit={handleSearch}>
            <input
              type="text"
              placeholder="Enter city"
              value={city}
              onChange={(e) => setCity(e.target.value)}
              style={{ padding: "10px", marginRight: "10px" }}
            />
            <button type="submit" style={{ padding: "10px" }}>
              Search
            </button>
          </form>
    
          {loading && <p>Loading...</p>}
          {error && <p style={{ color: "red" }}>{error}</p>}
    
          {weatherData && (
            <div style={{ marginTop: "20px" }}>
              <h2>Current Weather in {weatherData.city.name}</h2>
              <p>Temperature: {weatherData.list[0].main.temp} °C</p>
              <p>Weather: {weatherData.list[0].weather[0].description}</p>
    
              <h3>5-Day Forecast</h3>
              <div style={{ display: "flex", justifyContent: "center" }}>
                {weatherData.list.map((item, index) =>
                  // (every 3 hour forecasting) 24h/3h == 8 weather forecast for each day
                  // i.e. 0th index have first day weather forecast
                  // ==> hence next day weather forcast will lies on 8th index
                  index % 8 === 0 ? (
                    <div
                      key={index}
                      style={{
                        border: "1px solid #ccc",
                        padding: "10px",
                        margin: "5px",
                      }}
                    >
                      <p>{new Date(item.dt_txt).toLocaleDateString()}</p>
                      <p>Temp: {item.main.temp} °C</p>
                      <p>{item.weather[0].description}</p>
                    </div>
                  ) : null
                )}
              </div>
            </div>
          )}
        </div>
      );
    };
    
    export default WeatherApp;
    
    ```
</details>

<!-- TOC --><a name="problem-18-timer-with-startstopreset"></a>

### Problem 18: Timer with Start/Stop/Reset

**Description:**

- Create a timer that counts up from zero.
- Provide buttons to start, stop, and reset the timer.
- Display the elapsed time in a human-readable format (e.g., MM:SS).
<details>
  <summary> Solution: </summary>
    
    ```jsx
    
    import { useRef, useState } from "react";
    
    const padDigitWithZero = (number) => {
      return String(number).padStart(2, "0");
    };
    
    function convertSecondsToTime(seconds) {
      const h = Math.floor(seconds / 3600);
      const m = Math.floor((seconds % 3600) / 60);
      const s = seconds % 60;
    
      return `${padDigitWithZero(h)} : ${padDigitWithZero(m)} : ${padDigitWithZero(
        s
      )}`;
    }
    
    const initialTime = {
      timeString: "00 : 00 : 00",
      totalSeconds: 0,
    };
    
    const StopWatch = () => {
      const [currentTime, setCurrentTime] = useState(initialTime);
      const [isTimerRunning, setIsTimerRunning] = useState(false);
      const timerRef = useRef();
    
      const timerHandler = (isStartTimer) => {
        if (!isStartTimer) {
          clearInterval(timerRef.current);
          setIsTimerRunning(!isTimerRunning);
          return;
        }
    
        timerRef.current = setInterval(() => {
          setCurrentTime((prevTime) => {
            const updatedSeconds = prevTime.totalSeconds + 1;
            return {
              timeString: convertSecondsToTime(updatedSeconds),
              totalSeconds: updatedSeconds,
            };
          });
        }, 1000);
    
        setIsTimerRunning(!isTimerRunning);
      };
    
      const resetTimer = () => {
        clearInterval(timerRef.current);
        setIsTimerRunning(false);
        setCurrentTime(initialTime);
      };
    
      return (
        <div>
          <h3>
            {isTimerRunning
              ? "Running..."
              : currentTime.totalSeconds > 0 && "Paused..."}
          </h3>
          <div style={{ marginBottom: "1rem" }}>{currentTime.timeString}</div>
          <div style={{ display: "flex", gap: "1rem", justifyContent: "center" }}>
            <button onClick={() => timerHandler(!isTimerRunning)}>
              {isTimerRunning ? "Stop" : "Start"}
            </button>
            <button onClick={resetTimer}>Reset</button>
          </div>
        </div>
      );
    };
    
    export default StopWatch;
    
    ```
</details>

<!-- TOC --><a name="problem-19-dynamic-form-fields"></a>

### Problem 19: Dynamic Form Fields

**Description:**

- Create a form with dynamic fields.
- Allow the user to add or remove fields dynamically.
- Collect and display the form data upon submission.
<details>
  <summary> Solution: </summary>
    
    ```jsx
    import { useState } from "react";
    
    function DynamicFormFields() {
      const [formFields, setFormFields] = useState([{ name: "", age: "" }]);
      const [formData, setFormData] = useState(null);
    
      const handleFormChange = (event, index) => {
        let data = [...formFields];
        data[index][event.target.name] = event.target.value;
        setFormFields(data);
      };
    
      const addFields = () => {
        let object = {
          name: "",
          age: "",
        };
    
        setFormFields([...formFields, object]);
      };
    
      const removeFields = (index) => {
        let data = [...formFields];
        data.splice(index, 1);
        setFormFields(data);
      };
    
      const handleSubmit = (event) => {
        event.preventDefault();
        const data = formFields.map((field) => field);
        setFormData(data);
      };
    
      return (
        <div>
          <form onSubmit={handleSubmit}>
            {formFields.map((form, index) => {
              return (
                <div
                  key={index}
                  style={{
                    display: "flex",
                    gap: "1rem",
                    marginBottom: "1rem",
                    justifyContent: "center",
                  }}
                >
                  <input
                    name="name"
                    placeholder="Name"
                    onChange={(event) => handleFormChange(event, index)}
                    value={form.name}
                  />
                  <input
                    name="age"
                    placeholder="Age"
                    onChange={(event) => handleFormChange(event, index)}
                    value={form.age}
                  />
                  <button onClick={() => removeFields(index)}>Remove</button>
                </div>
              );
            })}
          </form>
    
          <button onClick={addFields} style={{ marginRight: "1rem" }}>
            Add More..
          </button>
          <button onClick={handleSubmit}>Submit</button>
    
          {formData && (
            <div style={{ marginTop: "20px" }}>
              <h3>Form Data:</h3>
              <pre>{JSON.stringify(formData, null, 2)}</pre>
            </div>
          )}
        </div>
      );
    }
    
    export default DynamicFormFields;
    
    ```
</details>

<!-- TOC --><a name="problem-192-dynamic-config-driven-form"></a>

### Problem 19.2: Dynamic Config Driven Form

<!-- TOC --><a name="problem-20-notification-system"></a>

### Problem 20: Notification System

**Description:**

- Implement a notification system that displays notifications at the top of the screen.
- Each notification should disappear automatically after a few seconds.
- Provide a close button to manually dismiss a notification.

<details>
  <summary> Solution: </summary>
    
    ```jsx
    // useToastNotification.jsx
    
    import { useCallback, useState } from "react";
    import Notification from "./Notification";
    import "./styles.css";
    
    const useToastNotification = () => {
      const [notifications, setNotifications] = useState([]);
    
      const removeNotifation = (id) => {
        setNotifications((prevNotifications) => {
          const updatedNotifaions = [...prevNotifications].filter(
            (notification) => notification.id !== id
          );
    
          return updatedNotifaions;
        });
      };
    
      const showNotifications = useCallback((notificationObject) => {
        const notificationId = Math.random().toString(32);
        setNotifications((prevNotifications) => [
          ...prevNotifications,
          { id: notificationId, ...notificationObject },
        ]);
    
        if (notificationObject.duration) {
          setTimeout(() => {
            removeNotifation(notificationId);
          }, notificationObject.duration);
        }
      }, []);
    
      const NotificationContainer = () => {
        return (
          <div className="toast-message-container">
            {notifications.map((notification) => {
              return (
                <Notification
                  key={notification.id}
                  {...notification}
                  onClose={removeNotifation}
                />
              );
            })}
          </div>
        );
      };
    
      return {
        showNotifications,
        NotificationsComponent: NotificationContainer,
      };
    };
    
    export default useToastNotification;
    ```
    
    ```jsx
    //notification.jsx
    const Notification = ({ id, message, onClose }) => {
      return (
        <div className="toast-message">
          <button onClick={() => onClose(id)}>&#10060;</button>
          <p>{message}</p>
        </div>
      );
    };
    
    export default Notification;
    ```
    
    ```jsx
    // **ToastNotificationDemo.jsx**
    /* 
    ### Problem 20: Notification System
    
    **Description:**
    
    - Implement a notification system that displays notifications at the top of the screen.
    - Each notification should disappear automatically after a few seconds.
    - Provide a close button to manually dismiss a notification.
    
    */
    
    import { useState } from "react";
    import useToastNotification from "./useToastNotification";
    
    const ToastNotificationDemo = () => {
      const [count, setCount] = useState(0);
    
      const { showNotifications, NotificationsComponent } = useToastNotification();
    
      return (
        <div>
          <NotificationsComponent />
          <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolor tenetur
            molestias exercitationem fugit iure minus earum sit fuga explicabo
            ratione alias delectus dolores soluta quasi, accusamus hic sed adipisci
            atque?
          </p>
          <button
            onClick={() => {
              setCount((prevCount) => prevCount + 1);
    
              showNotifications({
                message: `Toast Message ${count + 1}`,
                duration: 3000,
              });
            }}
          >
            Show Notification
          </button>
        </div>
      );
    };
    
    export default ToastNotificationDemo;
    ```
</details>

<!-- TOC --><a name="problem-21-traffic-light"></a>

### Problem 21: Traffic Light

**Description:**
Build a traffic light where the lights switch from green
to yellow to red after predetermined intervals and
loop indefinitely.
Each light should be lit for the following durations:

Red light: 4000ms
Yellow light: 5000ms
Green light: 3000ms
You are free to exercise your creativity to style the appearance of the traffic light.

<details>
  <summary> Solution: </summary>

```jsx
/**
 * 
Build a traffic light where the lights switch from green
to yellow to red after predetermined intervals and 
loop indefinitely.
 Each light should be lit for the following durations:

Red light: 4000ms
Yellow light: 5000ms
Green light: 3000ms
You are free to exercise your creativity to style the appearance of the traffic light.
 */

import React, { useState, useEffect } from "react";
import "./styles.css"; // Create a CSS file for styling

const lightConfig = {
  red: { color: "yellow", time: 500 },
  yellow: { color: "green", time: 3000 },
  green: { color: "red", time: 4000 },
};

const TrafficLight = () => {
  const [light, setLight] = useState(lightConfig["red"]);

  useEffect(() => {
    const interval = setTimeout(() => {
      setLight((prevValue) => {
        return lightConfig[prevValue.color];
      });
    }, light.time);

    return () => {
      clearInterval(interval);
    };
  }, [light]);

  return (
    <div className="traffic-light">
      <div
        className="light"
        style={{ backgroundColor: light.color === "red" ? "red" : "white" }}
      ></div>
      <div
        className="light"
        style={{
          backgroundColor: light.color === "yellow" ? "yellow" : "white",
        }}
      ></div>
      <div
        className="light"
        style={{
          backgroundColor: light.color === "green" ? "green" : "white",
        }}
      ></div>
    </div>
  );
};

export default TrafficLight;
```

styles.css

```css
.traffic-light {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100px;
  background-color: #333;
  border-radius: 10px;
  padding: 10px;
}

.light {
  width: 60px;
  height: 60px;
  margin: 10px 0;
  border-radius: 50%;
  background-color: #333;
  opacity: 0.4;
  transition: opacity 0.5s ease;
}
```

</details>

<!-- TOC --><a name="problem-22-create-google-calculator-app"></a>

### Problem 22: Create Google calculator app

- **Description**

  Example 1:
  input : (2+3*5+9*5)/2
  output : 31
  Example 2:
  input : 1+2+(6/2*50%154)+4*2
  output : 242
  Create an calculator app in react as same as google calculator

<details>
  <summary> Solution: </summary>

    - 1st : using Function constructor

        To create a calculator app in React with a UI similar to the attached image and functionality like Google's calculator, we need to handle basic arithmetic operations and evaluate expressions correctly, including operator precedence and parentheses.

        Here’s the code to implement such a calculator in React:

        ### Calculator Component

        ```jsx
        import React, { useState } from "react";
        import "./styles.css";

        const Calculator1 = () => {
          const [expression, setExpression] = useState("");
          const [result, setResult] = useState("");

          const handleButtonClick = (value) => {
            setExpression((prev) => prev + value);
          };

          const handleClear = () => {
            setExpression("");
            setResult("");
          };

          const handleBackspace = () => {
            setExpression((prev) => prev.slice(0, -1));
          };

          const handleEqual = () => {
            try {
              // Evaluate the expression using the Function constructor
              const evalResult = Function(`"use strict"; return (${expression})`)();
              setResult(evalResult);
            } catch (error) {
              setResult("Error");
            }
          };

          return (
            <div className="calculator">
              <div className="display">
                <textarea
                  className="expression"
                  onChange={(e) => setExpression(e.target.value)}
                  value={expression}
                  onKeyDown={(e) => {
                    if (e.key === "Enter") {
                      handleEqual();
                    }
                  }}
                >
                  {expression}
                </textarea>
                <div className="result">{result}</div>
              </div>
              <div className="buttons">
                <button onClick={handleClear}>AC</button>
                <button onClick={() => handleButtonClick("%")}>%</button>
                <button onClick={handleBackspace}>⌫</button>
                <button onClick={() => handleButtonClick("/")}>÷</button>
                <button onClick={() => handleButtonClick("7")}>7</button>
                <button onClick={() => handleButtonClick("8")}>8</button>
                <button onClick={() => handleButtonClick("9")}>9</button>
                <button onClick={() => handleButtonClick("*")}>×</button>
                <button onClick={() => handleButtonClick("4")}>4</button>
                <button onClick={() => handleButtonClick("5")}>5</button>
                <button onClick={() => handleButtonClick("6")}>6</button>
                <button onClick={() => handleButtonClick("-")}>-</button>
                <button onClick={() => handleButtonClick("1")}>1</button>
                <button onClick={() => handleButtonClick("2")}>2</button>
                <button onClick={() => handleButtonClick("3")}>3</button>
                <button onClick={() => handleButtonClick("+")}>+</button>
                <button onClick={() => handleButtonClick("00")}>00</button>
                <button onClick={() => handleButtonClick("0")}>0</button>
                <button onClick={() => handleButtonClick(".")}>.</button>
                <button onClick={handleEqual}>=</button>
              </div>
            </div>
          );
        };

        export default Calculator1;

        ```

        ### CSS for Styling (styles.css)

        ```css
        .calculator {
          width: 300px;
          margin: 0 auto;
          padding: 10px;
          border: 2px solid #000;
          border-radius: 10px;
          background-color: #333;
          color: #fff;
        }

        .display {
          height: 100px;
          padding: 10px;
          margin-bottom: 10px;
          border: 1px solid #000;
          border-radius: 5px;
          background-color: #222;
          color: #fff;
          font-size: 24px;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
        }

        .expression {
          font-size: 18px;
          color: #aaa;
        }

        .result {
          font-size: 24px;
          color: #fff;
          text-align: right;
        }

        .buttons {
          display: grid;
          grid-template-columns: repeat(4, 1fr);
          gap: 10px;

          button {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            border: 1px solid #000;
            border-radius: 5px;
            background-color: #444;
            color: #fff;
            cursor: pointer;
          }

          button:hover {
            background-color: #555;
          }
        }

        ```

        ### Explanation

        1. **State Management**:
            - `expression`: Stores the current input expression.
            - `result`: Stores the calculated result.
        2. **Button Handlers**:
            - `handleButtonClick`: Appends the clicked button value to the expression.
            - `handleClear`: Clears the expression and result.
            - `handleBackspace`: Removes the last character from the expression.
            - `handleEqual`: Evaluates the expression using the `Function` constructor.
        3. **Rendering**:
            - The display area shows the current expression and result.
            - Buttons are rendered in a grid layout, similar to the calculator image.

        You can add more styles to match the exact look of the provided image and further refine the functionality if needed.

    - 2nd: using [mathjs](https://mathjs.org/) library

        you can evaluate mathematical expressions safely without using the `Function` constructor or `eval` by implementing a parser or using an existing library. One such library is [mathjs](https://mathjs.org/), which is designed for safely evaluating mathematical expressions.

        Here's how you can modify your calculator app to use `mathjs`:

        ### 1. Install mathjs

        First, install the mathjs library:

        ```bash
        npm install mathjs

        ```

        ### 2. Update Calculator Component

        Now, update your `Calculator` component to use `mathjs` for expression evaluation.

        ```jsx
        import React, { useState } from "react";
        import { create, all } from "mathjs";
        import "./styles.css";

        // Configure mathjs
        const math = create(all);

        const Calculator2 = () => {
          const [expression, setExpression] = useState("");
          const [result, setResult] = useState("");

          const handleButtonClick = (value) => {
            setExpression((prev) => prev + value);
          };

          const handleClear = () => {
            setExpression("");
            setResult("");
          };

          const handleBackspace = () => {
            setExpression((prev) => prev.slice(0, -1));
          };

          const handleEqual = () => {
            try {
              // Evaluate the expression using mathjs
              const evalResult = math.evaluate(expression);
              setResult(evalResult);
            } catch (error) {
              setResult("Error");
            }
          };

          return (
            <div className="calculator">
              <div className="display">
                <textarea
                  className="expression"
                  onChange={(e) => setExpression(e.target.value)}
                  value={expression}
                  onKeyDown={(e) => {
                    if (e.key === "Enter") {
                      handleEqual();
                    }
                  }}
                >
                  {expression}
                </textarea>
                <div className="result">{result}</div>
              </div>
              <div className="buttons">
                <button onClick={handleClear}>AC</button>
                <button onClick={() => handleButtonClick("%")}>%</button>
                <button onClick={handleBackspace}>⌫</button>
                <button onClick={() => handleButtonClick("/")}>÷</button>
                <button onClick={() => handleButtonClick("7")}>7</button>
                <button onClick={() => handleButtonClick("8")}>8</button>
                <button onClick={() => handleButtonClick("9")}>9</button>
                <button onClick={() => handleButtonClick("*")}>×</button>
                <button onClick={() => handleButtonClick("4")}>4</button>
                <button onClick={() => handleButtonClick("5")}>5</button>
                <button onClick={() => handleButtonClick("6")}>6</button>
                <button onClick={() => handleButtonClick("-")}>-</button>
                <button onClick={() => handleButtonClick("1")}>1</button>
                <button onClick={() => handleButtonClick("2")}>2</button>
                <button onClick={() => handleButtonClick("3")}>3</button>
                <button onClick={() => handleButtonClick("+")}>+</button>
                <button onClick={() => handleButtonClick("00")}>00</button>
                <button onClick={() => handleButtonClick("0")}>0</button>
                <button onClick={() => handleButtonClick(".")}>.</button>
                <button onClick={handleEqual}>=</button>
              </div>
            </div>
          );
        };

        export default Calculator2;
        ```

        ### CSS for Styling (styles.css)

        This remains the same:

        ```css
        .calculator {
          width: 300px;
          margin: 0 auto;
          padding: 10px;
          border: 2px solid #000;
          border-radius: 10px;
          background-color: #333;
          color: #fff;
        }

        .display {
          height: 100px;
          padding: 10px;
          margin-bottom: 10px;
          border: 1px solid #000;
          border-radius: 5px;
          background-color: #222;
          color: #fff;
          font-size: 24px;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
        }

        .expression {
          font-size: 18px;
          color: #aaa;
        }

        .result {
          font-size: 24px;
          color: #fff;
          text-align: right;
        }

        .buttons {
          display: grid;
          grid-template-columns: repeat(4, 1fr);
          gap: 10px;

          button {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            border: 1px solid #000;
            border-radius: 5px;
            background-color: #444;
            color: #fff;
            cursor: pointer;
          }

          button:hover {
            background-color: #555;
          }
        }

        ```

        ### Explanation

        1. **Mathjs Integration**:
            - The `mathjs` library is used to evaluate the mathematical expression.
            - The `math.evaluate(expression)` method safely evaluates the string expression.
        2. **State Management**:
            - `expression`: Stores the current input expression.
            - `result`: Stores the calculated result.
        3. **Button Handlers**:
            - `handleButtonClick`: Appends the clicked button value to the expression.
            - `handleClear`: Clears the expression and result.
            - `handleBackspace`: Removes the last character from the expression.
            - `handleEqual`: Evaluates the expression using `mathjs`.

        This implementation avoids the security issues associated with `eval` and `Function` while providing the same functionality.

    - 3rd: by implementing a basic expression parser.

        it is possible to evaluate mathematical expressions without using external libraries by implementing a basic expression parser. Here's how you can build a simple calculator in React without relying on `eval` or external libraries:

        ### Calculator Component

        ```jsx
        import React, { useState } from "react";
        import "./styles.css";

        const Calculator3 = () => {
          const [expression, setExpression] = useState("");
          const [result, setResult] = useState("");

          const handleButtonClick = (value) => {
            setExpression((prev) => prev + value);
          };

          const handleClear = () => {
            setExpression("");
            setResult("");
          };

          const handleBackspace = () => {
            setExpression((prev) => prev.slice(0, -1));
          };

          const handleEqual = () => {
            try {
              // Evaluate the expression safely
              const evalResult = evaluateExpression(expression);
              setResult(evalResult);
            } catch (error) {
              setResult("Error");
            }
          };

          const evaluateExpression = (expr) => {
            // Parse the expression and compute the result
            // Handle basic operators: +, -, *, /, %, (, )
            const operators = {
              "+": (a, b) => a + b,
              "-": (a, b) => a - b,
              "*": (a, b) => a * b,
              "/": (a, b) => a / b,
              "%": (a, b) => a % b,
            };

            const precedence = {
              "+": 1,
              "-": 1,
              "*": 2,
              "/": 2,
              "%": 2,
            };

            const toPostfix = (infix) => {
              const output = [];
              const operatorsStack = [];
              const tokens = infix.match(/\d+(\.\d+)?|[+\-*/%()]/g);

              tokens.forEach((token) => {
                if (!isNaN(token)) {
                  output.push(Number(token));
                } else if (token in operators) {
                  while (
                    operatorsStack.length &&
                    precedence[token] <=
                      precedence[operatorsStack[operatorsStack.length - 1]]
                  ) {
                    output.push(operatorsStack.pop());
                  }
                  operatorsStack.push(token);
                } else if (token === "(") {
                  operatorsStack.push(token);
                } else if (token === ")") {
                  while (operatorsStack[operatorsStack.length - 1] !== "(") {
                    output.push(operatorsStack.pop());
                  }
                  operatorsStack.pop();
                }
              });

              while (operatorsStack.length) {
                output.push(operatorsStack.pop());
              }

              return output;
            };

            const evaluatePostfix = (postfix) => {
              const stack = [];

              postfix.forEach((token) => {
                if (typeof token === "number") {
                  stack.push(token);
                } else {
                  const b = stack.pop();
                  const a = stack.pop();
                  stack.push(operators[token](a, b));
                }
              });

              return stack[0];
            };

            const postfix = toPostfix(expr);
            return evaluatePostfix(postfix);
          };

          return (
            <div className="calculator">
              <div className="display">
                <textarea
                  className="expression"
                  onChange={(e) => setExpression(e.target.value)}
                  value={expression}
                  onKeyDown={(e) => {
                    if (e.key === "Enter") {
                      handleEqual();
                    }
                  }}
                >
                  {expression}
                </textarea>
                <div className="result">{result}</div>
              </div>
              <div className="buttons">
                <button onClick={handleClear}>AC</button>
                <button onClick={() => handleButtonClick("%")}>%</button>
                <button onClick={handleBackspace}>⌫</button>
                <button onClick={() => handleButtonClick("/")}>÷</button>
                <button onClick={() => handleButtonClick("7")}>7</button>
                <button onClick={() => handleButtonClick("8")}>8</button>
                <button onClick={() => handleButtonClick("9")}>9</button>
                <button onClick={() => handleButtonClick("*")}>×</button>
                <button onClick={() => handleButtonClick("4")}>4</button>
                <button onClick={() => handleButtonClick("5")}>5</button>
                <button onClick={() => handleButtonClick("6")}>6</button>
                <button onClick={() => handleButtonClick("-")}>-</button>
                <button onClick={() => handleButtonClick("1")}>1</button>
                <button onClick={() => handleButtonClick("2")}>2</button>
                <button onClick={() => handleButtonClick("3")}>3</button>
                <button onClick={() => handleButtonClick("+")}>+</button>
                <button onClick={() => handleButtonClick("00")}>00</button>
                <button onClick={() => handleButtonClick("0")}>0</button>
                <button onClick={() => handleButtonClick(".")}>.</button>
                <button onClick={handleEqual}>=</button>
              </div>
            </div>
          );
        };

        export default Calculator3;

        ```

        ### CSS for Styling (styles.css)

        ```css
        .calculator {
          width: 300px;
          margin: 0 auto;
          padding: 10px;
          border: 2px solid #000;
          border-radius: 10px;
          background-color: #333;
          color: #fff;
        }

        .display {
          height: 100px;
          padding: 10px;
          margin-bottom: 10px;
          border: 1px solid #000;
          border-radius: 5px;
          background-color: #222;
          color: #fff;
          font-size: 24px;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
        }

        .expression {
          font-size: 18px;
          color: #aaa;
        }

        .result {
          font-size: 24px;
          color: #fff;
          text-align: right;
        }

        .buttons {
          display: grid;
          grid-template-columns: repeat(4, 1fr);
          gap: 10px;

          button {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            border: 1px solid #000;
            border-radius: 5px;
            background-color: #444;
            color: #fff;
            cursor: pointer;
          }

          button:hover {
            background-color: #555;
          }
        }

        ```

        ### Explanation

        1. **State Management**:
            - `expression`: Stores the current input expression.
            - `result`: Stores the calculated result.
        2. **Button Handlers**:
            - `handleButtonClick`: Appends the clicked button value to the expression.
            - `handleClear`: Clears the expression and result.
            - `handleBackspace`: Removes the last character from the expression.
            - `handleEqual`: Evaluates the expression using a custom parser.
        3. **Expression Evaluation**:
            - The `evaluateExpression` function converts the infix expression to postfix notation using the Shunting Yard algorithm.
            - The postfix expression is then evaluated to get the result.

        By implementing a custom parser, you avoid the security issues associated with `eval` and `Function`. This implementation handles basic arithmetic operations and respects operator precedence.

</details>

<!-- TOC --><a name="problem-23-selectable-grid"></a>

### Problem 23: Selectable grid

<!-- TOC --><a name="problem-24-multi-select-search"></a>

### Problem 24: Multi-Select-Search

<!-- TOC --><a name="problem-25-pagination"></a>

### Problem 25: Pagination

<!-- TOC --><a name="problem-25-smart-search"></a>

### Problem 25: Smart-Search

<!-- TOC --><a name="problem-26-stepper"></a>

### Problem 26: Stepper

<!-- TOC --><a name="other-repos-for-react-coding-challenges"></a>

### Other Repos for react coding challenges

https://github.com/codewithayaann/Machine-Round-Challenge

https://github.com/NikhilJHA01/Machine-Coding-Round

https://github.com/sameerkali/React_coding_round_practice

https://workat.tech/machine-coding/article/how-to-practice-for-machine-coding-kp0oj3sw2jca
