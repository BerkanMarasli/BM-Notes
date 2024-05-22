<h1> Promises </h1>

ColorCode's YouTube video on promises: https://www.youtube.com/watch?v=TnhCX0KkPqs



<u>Background</u>

Sync (blocking) vs Async (not-blocking)

The problem with Async: what happens when you need to follow the async process and do a task with the result of the async function?



<u>Syntax</u>

```typescript
new Promise((resolve, reject) => {
  // async logic goes here
})

// use resolve for the success case
new Promise((resolve, reject) => {
  resolve()
})

// use reject for the failure case
new Promise((resolve, reject) => {
  reject()
})
```



<u>Different phases of a promise</u>

- Pending
- Fulfiled or resolved
- Rejected



<u>Example of using promise</u>

```typescript
const getWeather = () => {
  return new Promise((resolve, reject) => {})
}

const promise = getWeather() // Promise { <pending> }

//=============================================================

const getWeather = () => {
  return new Promise((resolve, reject) => {
    resolve('Sunny')
  })
}

// .then() is called when the promise settles
promise.then((data) => console.log(`Resolved: ${data}`)) // 'Resolved: Sunny'

//=============================================================

const getWeather = () => {
  return new Promise((resolve, reject) => {
    reject('Sunny')
  })
}

// .then(resolveFunc, rejectFunc)
promise.then(
  (data) => console.log(`Resolved: ${data}`),
  (data) => console.log(`Rejected: ${data}`)
) // 'Rejected: Sunny'

//=============================================================
```

Clean up example:

```typescript
const getWeather = () => {
  return new Promise((resolve, reject) => {
    resolve('Sunny')
  })
}

const onSuccess = (data) => console.log(`Success: ${data}`)

const onError = (error) => console.log(`Error: ${error}`)

getWeather().then(onSuccess, onError) // 'Success: Sunny'
```



<u>Chaining</u>

```typescript
const getWeather = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Sunny')
    }, 1000)
  })
}

// The resolved value from getWeather() is chained in as the argument for getWeatherIcon()
const getWeatherIcon = (weather) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      switch(weather) {
        case 'Sunny':
          resolve('Hi Sunny')
          break
        case 'Cloudy':
          resolve('Hi Cloudy')
          break
        default:
          reject('NO ICON FOUND')
      }
    }, 1000)
  })
}

const onSuccess = (data) => console.log(`Success: ${data}`)

const onError = (error) => console.log(`Error: ${error}`)

getWeather().then(getWeatherIcon).then(onSuccess, onError) // 'Hi Sunny'
```



<u>Benefits of Promises over Callbacks</u>

- Avoids 'callback hell' and is easier to read:

```typescript
getLocationName((locationName) => {
  getLocationLatLon(locationName, (latLon) => {
    getWeather(latLon, (weatherData) => {
      getWeatherIcon(weatherData, (weatherIcon) => {
        displayWeatherIcon(weatherIcon)
      })
    })
  })
})

vs

getLocationName()
.then(getLocationLatLon)
.then(getWeather)
.then(getWeatherIcon)
.then(displayWeatherIcon)
```

- There's no callback function being passed into the next function, just the data. The function signatures become smaller and easier to read:

```typescript
getWeatherIcon(data, (icon) => {
  ...
})

vs

getWeatherIcon(data) {
  ...
}
```

- Handling of error scenarios



<u>Error handling: reject and .catch()</u>

You can handle the rejected value via the 2nd parameter in .then() or use .catch()

```typescript
const onSuccess = (data) => console.log(`Success: ${data}`)

const onError = (error) => console.log(`Error: ${error}`)

getWeather()
  .then(onSuccess)
  .catch(onError)
```

If the .then() is used to handle errors, the flow of code will mean the chained .then() statements will be executed. Whereas, if .catch() is used, this will skip all .then() statements that follow the promise rejection.



<u>.finally()</u>

Final piece of code to be run once every promise has been resolved or rejected:

```typescript
getWeather()
  .then(onSuccess)
  .catch(onError)
  .finally(() => console.log('FINALLY'))
```









