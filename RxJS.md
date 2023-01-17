# RxJS

## Operators

Operators allow us to create new observable streams, from previous stream(s) or from scratch, by applying some logic. This is a part of coding reactively. The alternative is to subscribe to the stream and putting out the values to be modified outside the stream. The idea is to keep the values in the stream until you want to use or display the values in the stream.

Interactive diagrams: https://rxmarbles.com/#map

---

### map

```
map(x => x * 10)
Input stream: 1,2,3
Output stream: 10,20,30
```

### filter

```
filter(x => x > 10)
Input stream: 2,30,22,5,60,1
Output stream: 30,22,60
```

*** Remember that these only operator on stream emissions. So, if the input stream outputted an array, you would need to map through the array using a standard array map method to access each value within the array.

---

### tap



