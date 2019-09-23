# General Assembly Project 2 - D&D Spellbook

[See the app in action!](https://herrchick.github.io/ga-project-2-react-api/#/)

##Introduction
For my second project at general assembly, we were asked to build a React.js app using a 3rd party API, pair coded in 48 hours.

We had decided on writing an app relevant to our mutual interests, and one which may prove useful to us in the future, a D&D spellbook.

### Technologies Used:
* React.js
* axios (for API requests)
* Bulma (CSS framework)
* webpack
* lodash (for filtering)
* Open5e API

### What we wanted to achieve:
If you have ever played D&D (you're on github, you probably have), you know that one of the most annoying parts of playing a magic user is having to search through the rulebook to search for the details of your spells; how much damage they do, dice rolls needed, and other rules. Our idea was to streamline this process by:

* Providing an easily searchable spellbook allowing users to filter by classtype, spell level, and school of magic
* A simple, but effective user interface using a virtual card system
* To speed up games by providing the user with all relevant information quickly.

## Features

### Filtering

Filtering on the index page was mostly performed by this function:

```javascript
filterCards() {
  const formData = this.state.formData
  const re = new RegExp(formData.search, 'i')
  const filterCards = _.filter(this.state.spells, card => {
    return (
      (!formData.level || card.level === formData.level) &&
      (!formData.class || card.dnd_class.includes(formData.class)) &&
      (!formData.school || card.school === formData.school) &&
      (re.test(card.name) || re.test(card.school) || re.test(card.dnd_class))
    )
  })
  // const sortedCards = _.orderBy(filterCards, [field], [order])
  return filterCards
}
```
Allowing us to store the saved search terms in our select fields. This data is then used to filter our spells array using lodash's filter function.

### Definitions via Modals
Bulma handily has modals built into the framework, allowing us to provide quick, pop up definitions of perhaps unknown words and rules to new players. When one of these words are clicked, the modal state is switched between active and not active:

```javascript
toggleModal(identify) {
  const desc = descriptions.classes[identify] || descriptions.schools[identify]
  if (!this.state.modalActive) {
    this.setState({modalActive: 'is-active', modalDesc: desc})
  } else {
    this.setState({modalActive: ''})
  }
}
```

### Images via google image search API
Open5e does not have any images for any of the spells in the spellbook, and we felt that an image would be handy for each spell detail page; but didn't want to find over 300 images by hand. Using the google images API and a plugin called "Google Images"  on NPM, each spell detail page is auto populated with the same keyword as the name of the spell from google images.

```javascript
getImage() {

  const client = new GoogleImages('004991023930242296851:9-esw8ey0xs', process.env.GOOGLE_API_KEY)
  client.search(`illustration ${this.state.spell.name}`)
    .then(images => this.setState({ img: images[0].url }))
    // .catch(err => console.log(err))
}
```

## Screenshots
![Index page](https://i.imgur.com/a7d1PU2.png)
![Modal](https://i.imgur.com/Zst7Egg.png)
![Show page](https://i.imgur.com/QW1FuT3.png)
