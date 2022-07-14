# Project 3: Plantpedia

## Overview
This was the third project for the Software Engineering Immersive course with GA, which consisted of a full-stack group project built using the MERN stack.

The assignment was to create a full stack website consisting of a backend that uses express to store user-generated data on a mongodb database and a frontend that uses React and React-based frameworks. The project was to be completed by a three-person team within one week.

Are you curious to see the end result? [Check out the site.](https://plant-pedia.herokuapp.com/) 

### Brief
* A full stack website that stores user-generated data in a mongodb database.
* A React frontend that is connected to the backend.
* User registration, login, and authentication.
* Complete it in one week.


### Collaborators
* Jacqueline Zhou - [Github](https://github.com/jacqizee/)
* Philip Sopher - [Github](https://github.com/psopher/)
* Rob Green - [Github](https://github.com/greezyBob/)


### Technologies Used
 MERN Stack (MongoDB, Express.js, React, Node.js)
* React Router
* JavaScript (ES6+)
* Mongoose
* JSON Web Token / bcrypt
* Material UI (MUI)
* HTML5, CSS3, and SASS
* Axios
* VSCode
* Eslint
* Git & GitHub
* Insomnia
* Trello for Project Management
* Third Party Dependencies:
  - react-image-crop
  - moment.js

### Team Planning
My team members and I used Trello to organise tasks that needed to be completed, and would assign tasks to one another based on how much progress we had made on the tasks of the previous day. We also had a daily stand up to discuss our challenges and our wins from the previous day, allowing us to share our opinions and think through any problems as a team. While we coded separately for quite a lot of the front-end, we worked alongside one another via Zoom so that in the case any of us ran into any issues or had any questions, the rest of us were right there and available to chip in and help out.

## Coding
We utilised Trello to divide up different parts of the application, while also creating a list of any bugs or features we noticed that should be addressed.

### Back-End
We initially worked on coding out the back-end of our project in a group of three. As we continued to develop the application, we added and updated the back-end as needed.


### Frontend
We decided to divide up different front-end pages across the three of us. We coded alongside one another on Zoom so that in any instance one of us ran into a bug or needed help, the rest of us were readily available to chip in and provide suggestions or talk through potential ways to tackle a problem.

Division of Work:
* Jackie - Add/Edit Plant Pages (form inputs and submission, metric/imperial measurement slider, styling), Plant Show Page (comment filtering, pagination, and styling), dark mode configuration, deployment
* Philip - Login/Register pages, User Profile, Edit User Profile (profile picture upload, image handling), Add/Edit Plant Pages (upload image feature)
* Rob - Homepage (search bar, flower colour filter, styling), Plant Show Page (favoriting, base styling, comment section UX), dark mode setup


When the MVP was made, we added bells and whistles to it, such as an intelligent search bar, comments, an editor application, and intelligent image handling and cropping.

Once the site had all the functionality we wanted, styling became the focus. We added a dark mode, a custom colour palette, and custom fonts, along with general stylistic enhancements so that it rendered well on mobile as well as on desktop.


### Searchbar
We decided we wanted the search bar to be able to filter for multiple flower colours at the same time and work in conjunction with searching by name. This meant I needed to keep track of what filters the user had applied and loop through the plant data to match only plants that met the specific criteria
```
useEffect(() => {
    //regexp search term for testing
    const regExpSearch = new RegExp(filters.searchTerm, 'i')
    regExpSearch.lastIndex = 0
    const filteredArray = plants.filter(plant => regExpSearch.test(plant.name))

    //if there are colors in the filters obj
    const colorFilterArray = []

    if (filters.flowerColorFilter.length) {
      filters.flowerColorFilter.forEach(color => {
        (filteredArray.length ? filteredArray : plants).forEach(plant => {
          if (plant.flowerColor.includes(color) && !colorFilterArray.includes(plant)) {
            colorFilterArray.push(plant)
          }
        })
      })
      setFilteredPlants(colorFilterArray)
    } else {
      //if just search term
      setFilteredPlants(filteredArray)
    }

  }, [filters, plants])
```
### Favouriting
In order to make favourites work I had to send the request to the back-end adding the plant to the users favourites and then request the plant data to be sent back to the user so it could be rerendered with the favourite icon updated.
```
//update state when user clicks fav icon and on page render
  useEffect(() => {
    const isFavorite = (singlePlant) => {
      if (!payload || !singlePlant) return
      setFavorite(singlePlant.favorites.includes(payload.sub))
    }
    isFavorite(plant)
  }, [plant])

  //send request to favorites
  const toggleFavorite = async (plant) => {
    if (userIsAuthenticated()) {
      try {
        await axios.put(`/api/plants/${plant._id}/favorite`, null, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        })
        favorite ? setFavorite(false) : setFavorite(true)
      } catch (error) {
        console.log(error)
      }
    } else {
      navigate('/login')
    }
  }
```

### Screenshots
![Homepage](/client/src/images/home-screen.png)
![User Profile](/client/src/images/user-profile.png)
![Show Page](/client/src/images/show-page.png)
![Mobile and Dark Mode](/client/src/images/mobile-dark.png)


### Challenges
I found the project very fun and insightful. It allowed us to put parts of what we had learned throughout the course to use, but also presented us with real world problems and tackling creating a project under a short deadline.
* When designing a database, it's important to be mindful of how we plan to use the data, and design accordingly
  * Keep things simple - only nest data when you have a real reason to do so 
* Clean, commented code is valuable to help both your team and yourself understand your code
* Planning is important and will help you to catch and solve problems.


### Wins/Learnings
* Building a fully functioning full stack app.
* Managing remote repos with git, including how to address merge conflicts
* Working in a team on a project, reading and working with someone else's code at times, as well as discussing ways to tackle bugs/problems


### Ideas for Future Improvements
* More descriptive error handling on forms
* Favoriting plants directly from the homepage rather than just on the Plant Show page
* More robust filter options for our search bar
* Reply to comments
