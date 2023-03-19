<script setup lang="ts">

</script>

<template>
<!-- Button trigger modal -->
<div class="infobutton">

    <img src="../assets/info-button.svg" alt="info" width="20" height="20" data-bs-toggle="modal" data-bs-target="#exampleModal" />

</div>


<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel" style="color: #ff8200">How Does It Work?</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
            <p>
                This site uses the <a href="https://en.wikipedia.org/wiki/Elo_rating_system" target="_blank">Elo Rating Algorithm</a> (the same system used to rank Chess players) to rank the majors.
                <br><br>
                The short of it is that the algorithm takes into account the difference in Elo Ratings between the two majors being compared.
                When a major wins a vote, it takes a certain amount of points from the other major, and adds that amount to its own.
                <br><br>
                A major with a higher Elo rating is more likely to win a vote than a major with a lower Elo rating.
                If the favored major wins, it will result in a lesser rating gain than if the underdog wins, and vice versa.
            </p>
      </div>
      <h5 class="modal-header modal-title" id="exampleModalLabel"></h5>
      <div class="modal-body">
      </div>
    </div>
  </div>
</div>
</template>

<style scoped>
@import '../assets/base.css';

h1 h2 h3 h4 h5 h6 
.heading{
    color: #000000;
}
p {
    color: var(--color-text-dark);
}
.infobutton {
    position: absolute;
    top: 0;
    right: 0;
    margin: 1rem;
    z-index: 5;
    cursor: pointer;
}
button {
  background: transparent;
  border: none !important;
  font-size:0;
}


</style>