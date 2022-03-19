---
title: Wir lieben es, mit dir zu sprechen!
description: "meta description"
contact_image: "author/martin.jpg"
draft: false

---


<!--more-->

{{<rawhtml>}}
<span>
<style> 
input[type=text] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
input[type=email] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
checkbox {
  width: auto;
  float: left;
  padding: 12px 20px;
  margin: 8px 4px;
  box-sizing: border-box;
}
textarea {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
button {
    background-color:#056478 !important;
    color:white;
    font-weight: bold;
    margin: 10px 10px 10px 0;
    font-size: 1.4rem;
    padding: 0 10px 0 10px;
    border-radius: 6px;
}
</style>
<form id="building" method="POST" data-netlify="true" action="/thanks">
  <p style="visibility:hidden;">
    <label>Don’t fill this out if you’re human: <input name="bot-field" /></label>
  </p>  
  <label for="name">Dein Name:</label>
  <input type="text" name="name" required>
  <label for="email">E-Mail-Adresse:</label>
  <input type="email" name="email" required>
  <label for="message">Deine Nachricht:</label>
  <textarea name="message" rows="10" cols="30" form="building"></textarea>  
  <input type="checkbox" name=privacy value="x" required> Ich stimme der Speicherung und Verarbeitung meiner persönlichen Daten durch die freie Schule Ostfriesland zu.<br/>
  <button type="submit">Absenden</button>
</form>
</span>
{{</rawhtml>}}