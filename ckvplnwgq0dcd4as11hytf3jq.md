## Stripe Integration with MERN Stack

In this post, I will go through the step-by-step process of how you can integrate one-time payment with the stripe in your MERN stack application.<br/> 
For simplicity, We will not go through the stripe signup, publishable and private keys. You can easily get them from the stripe website for the test or live environment.<br/>


# Overview
You send a request from your react frontend to your backend to create a payment intent for your customer. Payment intent is a process in which you specify details about your transaction like price, currency or email, etc.<br/>
The payment intent returns a **client_secret** that you will send as the response of your API. <br/>
On the frontend, you use the stripe confirm payment method with this **client_secret** to confirm the payment. If your payment is successful, it will return the status **succeeded**. 

# Setting up the backend
In your node js backend, you need to install the stripe dependency by typing the following command.</br>

```
npm i stripe
```
** Note: You need to get your publishable and private API keys from your stripe account. It is advised to use them as environment variables to make them secure. **
</br>After the installation, import the stripe into your file by the following code and instantiate it with your private API key.</br>
```
const stripe = require("stripe")(process.env.Private_Api_Key);
```
Now we build our API route to create the payment intent. Remember, we create our payment intents on the backend so that can confirm that the user is paying the right amount and cannot change it afterward.</br>
Let's create a simple route called the payment-intent,
```
app.post("/payment-intent", async(req,res)=>{
const amount =req.body
//enter your checks whether the payment is correct
})
```
Stripe has a method called ** paymentIntents.create **  and it takes an object as its parameters in which you specify the amount, currency, email, etc. There are a number of options in the object and you can read them in the docs.</br>
After verifying the amount you can use the following code to create a payment intent,
```
 const paymentIntent = await stripe.paymentIntents.create({
    amount ,
    currency: "usd",  
  });
```
The paymentIntent will have a ** client_secret ** that you need to send as a response to the frontend.
```
res.status(200).json(paymentIntent.client_secret);
```
That's it. You are all set up at the backend.

# Setting up React Js

Like in the backend, first you need to install the stripe dependencies. 
```
npm i @stripe/react-stripe-js @stripe/stripe-js
```
The stripe has a provider called ** Elements **. It allows you to use the stripe's prebuilt components and the Stripe Js object in any nested component. A method ** loadStripe ** asynchronously loads the Stripe.js script and initializes a Stripe object.  Pass the returned promise to the ** Elements ** wrapper.</br>
The ** loadStripe ** method is imported from `@stripe/stripe-js` and takes your stripe publishable key as a parameter.
```
import { Elements } from "@stripe/react-stripe-js";
import { loadStripe } from "@stripe/stripe-js";
const stripePromise = loadStripe(`${process.env.REACT_APP_PUBLISHABLE_KEY}`);

```
** Note: The loadStripe should be called outside the component's render.** 
 Wrap your checkout page with the `Elements` wrapper
```
<Elements stripe={stripePromise}>
        <CheckoutPage/>
      </Elements>
```
Stripe has a lot of prebuilt components for entering the car numbers, CVC, zip code, etc. They are easy to use and customizable. You can read the docs for further information.</br>
The Checkout page is a simple form with a submit function.
```
import {
  CardNumberElement,
  CardExpiryElement,
  CardCvcElement,
} from "@stripe/react-stripe-js";
const CheckoutPage=()=>{
const handlePayment= async()=>{
//payment functionality
}


return (
<form onSubmit={handlePayment}>
 <CardNumberElement/>
 <CardExpiryElement />
<CardCvcElement />    
 <button>Confirm Payment</button>
</form>
)}
```
There are two useful hooks from stripe called `useStripe` and `useElements`.  The useStripe hook reference the Stripe object in the `Elements` wrapper and useElements access the mounted element. You can check the docs to read more.</br>
So in our ** CheckoutPage ** component import these hooks
```
import { useStripe, useElements} from '@stripe/react-stripe-js'
```
 and create a reference variable for each of them.
```
  const stripe = useStripe();
  const elements = useElements();
```
Now let's complete the handlePayment method functionality. </br>
Remember, we need the create the ** paymentIntent ** first by sending the amount to our backends ** payment-intent ** routes. If the response is successful, we will receive a ** client_secret ** id. We will pass this id to the  `stripe.confirmCardPayment` method along with the payment method for payment confirmation.</br>

```
const handlePayment=async(event)=>{
event.preventDefault();
const response = await axios.post(
        `You_BACKEND_URL/payment-intent`,
        {
          amount: 200,
        },
      );
if (response.status === 200){
const confirmPayment = await stripe.confirmCardPayment(
          response.data.client_secret,
          {
            payment_method: {
              card: elements.getElement(CardNumberElement),
            },
          }
        );
if(confirmPayment.paymentIntent.status === "succeeded"){
 console.log('payment confirmed');
}
}}
```
Here is the complete `<CheckoutPage/>` component which you can use for your reference.
```

import {
  CardNumberElement,
  CardExpiryElement,
  CardCvcElement,
 useStripe, 
 useElements
} from "@stripe/react-stripe-js";
const CheckoutPage=()=>{

const stripe = useStripe();
const elements = useElements();
const handlePayment= async()=>{
event.preventDefault();
const response = await axios.post(
        `You_BACKEND_URL/payment-intent`,
        {
          amount: 200,
        },
      );
if (response.status === 200){
const confirmPayment = await stripe.confirmCardPayment(
          response.data.client_secret,
          {
            payment_method: {
              card: elements.getElement(CardNumberElement),
            },
          }
        );
if(confirmPayment.paymentIntent.status === "succeeded"){
 console.log('payment confirmed');
}
}
}
return (
<form onSubmit={handlePayment}>
 <CardNumberElement/>
 <CardExpiryElement />
<CardCvcElement />    
 <button>Confirm Payment</button>
</form>
)}
```
Congratulations, You have successfully integrated the stripe payment gateway in your MERN stack application. 
