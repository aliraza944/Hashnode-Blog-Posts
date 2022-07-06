## Practical Async Requests in React

By practical I mean is how these async requests will work in a real-world app. In order to implement a great user experience, you need to properly handle the async errors, load time, network issues, etc. in a way that a normal user can understand. In this post, I will walk you through the possible scenarios you should handle while dealing with the async requests in your app:</br> 


- The request load time
- Error Returned Requests
- Request taking longer than expected

## The Request Load Time
Take a look at the code below

```
const handleAPIRequest = async () => {
    try {
        const response= await axios.get(`${your_api}`)
    } catch (error) {
        if(error)
        console.log(error);
    }
}
```
Let's say you have a button that has an `onClick` method that calls our `handleAPIRequest` function. This is an async request that will take some time depending upon the data, the internet connection, etc. The problem with that is when the request is on its way the user won't have any idea what is going on. Is the request in progress or failed? This will create a very bad user experience of your app.<br/>
You can simply improve the experience by adding some spinners or loaders once the request is in the process. What I like to do is to use a loading state which stays `true` if my request is on its way, and sets to false if the response has arrived or there is an error. <br/>
Take a look at this bare minimum code that uses this approach.
```
import React, {useState} from 'react';
import Spinner from "./Spinner"
const MyComponent = () => {
const [isLoading, setIsLoading] = useState(false)

const handleAPIRequest = async () => {
    try {
        setIsLoading(true)
        const response= await axios.get(`${your_api}`)
        if(response.status===200){
            setIsLoading(false)
            console.log(response);
        }
    } catch (error) {
        if(error)
        setIsLoading(false)
        console.log(error);
    }
}
  return <div>

{isLoading ? <Spinner /> : <p>{YOUR_API_RESPONSE}</p>}

  </div>;
};

export default MyComponent;


```
## Error Returned Requests
Handling errors returned in an API request in a proper way will greatly improve the user experience of your app. A properly handled error will let the users know what exactly went wrong and what the user can do to avoid it.<br/>
These errors are present in the `error` object of the `catch` block. They are distinguishable by the status codes and depending upon how the API is developed also contain the error messages. You can get the error codes from the `error.response.status` and display relevant messages accordingly.<br/>
Here is a bare minimum example of this approach
```
catch (error) {
        setIsLoading(false)
        if(error.response.status===400){
            alert('Email or password are wrong')
        }
        else if(error.response.status===401){
            alert("You are not authorized"); 
        }
    }

```
## Request taking longer than expected
It is quite possible that the user has a slow internet connection. In this case, if you use the above-mentioned `spinner` approach then the spinner will keep on circling, and it will appear that the app has frozen. To avoid this you can set a time after which if the request is still processing you can show a proper message to the users to let them know the request is processing and the app is not frozen.<br/> Here is the bare minimum code for this approach
```
const [isLoading, setIsLoading] = useState(false)
const [result, setResult] = useState()
const API_REQUEST_TIMEOUT=60*1000
const handleAPIRequest = async () => {
    try {
        setIsLoading(true)
        const response= await axios.get(`${your_api}`)
        if(response.status===200){
            setIsLoading(false)
            setResult(response)
        }
    } catch (error) {
        setIsLoading(false)
        if(error.response.status===400){
            alert('Email or password are wrong')
        }
        else if(error.response.status===401){
            alert("You are not authorized"); 
        }
setTimeout(()=>{
    if(!result){
        alert('This is taking so longer than usual...')
    }
}, API_REQUEST_TIMEOUT)


    }
}
```
##  Wrapping Up
In this post, you have gone through the possible use-cases of handling the API request in a way that improves the user experience of your app. By using spinner, proper messages, and using request timeouts you can let the user know what is going on and in case of an error how to fix it. 