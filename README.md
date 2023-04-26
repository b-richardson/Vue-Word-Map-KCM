
# Quick Vue Wordcloud

This is a simple Vue componenet which passes of an API request based on user selected parameters. After the request is made, a Wordcloud is generated in the DOM to visualize the frequency in which words appear in the content of blog posts!


## Authors

- Brian Richardson [@b-richardson](https://github.com/b-richardson)


## Run Locally

Clone the project

```bash
  git clone https://github.com/b-richardson/Vue-Word-Map-KCM.git
```

Go to the project directory

```bash
  cd word-cloud-kcm
```

Install dependencies

```bash
  npm install
```

Compile and Hot-Reload for Development

```bash
  npm run dev
```


## Optimizations

```What optimizations did you make in your code? E.g. refactors, performance improvements, accessibility```

Real optimization here would be filtering down the data from the blog posts more. Currently, with so many key value pairs the wordcloud is incredibly slow to generate.


## Documentation

The application has some basic parameters for the user to change. The word cloud is generated after the Search Button is pressed and data has been retrieved from the API.

There is a very large amount of data being input for the word cloud on most given queries so please note that it does take a little bit of time for the wordcloud to generate!

