---
title: "TIL: Positioning Images Next to a DIV"
layout: post
date: "2025-04-11"
permalink: posts/position-images-next-to-div/
categories: [TIL]
tags: [TIL, CSS]
img_path: /assets/images/position-images-next-to-div/
---

I am working on my CSS and SASS skills at the moment.

I am coding a multiplayer card game using React, Typescript for the frontend and Node.js for the backend.

I am chosing not to use a game engine or a HTML5 canvas for the game, instead I want to use CSS and SASS for styling the game.

The game is laid out with 4 players sitting in 4 seats across the card table.

In each seat I need to show the player's hand along with a player information panel showing the player name, the number of tricks they have, what team they are on and their team's score.

I also want to display a player avatar to the left of the player information panel and a waiting icon to the right of the player information panel if it is currently that player's turn.

## How I Achieved This

![Result](result.jpg)

### Setup parent div with relative positioning

```scss
.exp-card-table__player_info_container {
  position: relative;
}
```

```html
<div class="exp-card-table__player_info_container"></div>
```

### Add child elements

```scss
.exp-card-table__player_info_text {
  background-color: wheat;
  padding: 0.5em;
  border: 0.1em solid black;
  border-radius: 0.3em;
}
```

```html
<div class="exp-card-table__player_info_container">
  <div>
    <img alt="avatar" src="/avatars/default.svg" />
  </div>
  <div>
    <img alt="turn indicator" src="/turn.svg" />
  </div>
  <div class="exp-card-table__player_info_text">
    <p>s at seat 4</p>
    <p>Team: team2, Team Score: 0</p>
    <p>Tricks: 0</p>
  </div>
</div>
```

### Give div element you want shown on the left absolute positioning with a right value

```scss
.exp-card-table__player_info_avatar {
  position: absolute;
  right: 13.5em;
  width: 2.5em;
  margin-top: 0.5em;
}
```

```html
<div class="exp-card-table__player_info_container">
  <div class="exp-card-table__player_info_avatar">
    <img alt="avatar" src="/avatars/default.svg" />
  </div>
  <div>
    <img alt="turn indicator" src="/turn.svg" />
  </div>
  <div class="exp-card-table__player_info_text">
    <p>s at seat 4</p>
    <p>Team: team2, Team Score: 0</p>
    <p>Tricks: 0</p>
  </div>
</div>
```

### Give div element you want shown on the right absolute positioning with a left value

```scss
.exp-card-table__player_info_turn_indicator {
  position: absolute;
  left: 13.5em;
  width: 2.5em;
  margin-top: 0.5em;
}
```

```html
<div class="exp-card-table__player_info_container">
  <div class="exp-card-table__player_info_avatar">
    <img alt="avatar" src="/avatars/default.svg" />
  </div>
  <div class="exp-card-table__player_info_turn_indicator">
    <img alt="turn indicator" src="/turn.svg" />
  </div>
  <div class="exp-card-table__player_info_text">
    <p>s at seat 4</p>
    <p>Team: team2, Team Score: 0</p>
    <p>Tricks: 0</p>
  </div>
</div>
```

## Further Reading

Some great resources I recommend for upskilling in CSS and SASS are:

1. Advanced CSS and Sass: Flexbox, Grid, Animations and More! [Udemy](https://www.udemy.com/course/advanced-css-and-sass)
1. Conquering Responsive Layouts [kevinpowell.co](https://courses.kevinpowell.co/view/courses/conquering-responsive-layouts)
1. Kevin Powell's YouTube Channel [YouTube](https://www.youtube.com/@KevinPowell)
1. w3schools CSS Tutorial [w3schools.com](https://www.w3schools.com/css/default.asp)
