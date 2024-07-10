<script setup>
import { inject } from "vue";

const materials = inject("materials");

const combined_materials = Object.values(materials)
  .filter((item) => Array.isArray(item)) // Nur Arrays filtern
  .flat();
</script>

<template>
  <!-- <div class="container"> -->
  <div class="con1">
    <img
      v-for="material in combined_materials"
      :src="`../src/assets/materials/${material.name}.png`"
      alt=""
    />
  </div>
  <div class="con2">
    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Quaerat ratione
      perspiciatis voluptatem accusamus sequi, qui quidem maxime quasi, sapiente
      praesentium nemo temporibus laborum obcaecati? Fugit quis omnis impedit,
      ad deleniti cumque libero necessitatibus voluptatibus minus quam! Alias
      numquam, maiores consequatur perspiciatis optio placeat distinctio ipsam
      nobis iste, quidem commodi molestias. Illum accusantium impedit aperiam
      natus, vel, corporis modi labore asperiores numquam error blanditiis
      dolorem quis illo laudantium. Laborum amet autem praesentium blanditiis a
      sequi nobis veniam, quia laudantium nemo, qui ad ipsa iusto reprehenderit,
      ab architecto minus molestiae voluptatum dolore atque? Inventore
      voluptatum repudiandae sunt adipisci iure porro numquam obcaecati.
      Suscipit optio eum quaerat eligendi earum nobis ex temporibus voluptatum
      nam fugit quas culpa cum facilis ullam libero aut natus distinctio ratione
      quo eos obcaecati, minus doloribus labore. Aut, dolor est ab voluptas
      tempore dicta pariatur modi, quia, voluptates obcaecati repudiandae
      repellat fuga unde reiciendis asperiores accusamus facilis in hic incidunt
      dolorem quas! Illo pariatur exercitationem numquam sequi ratione id
      voluptatem neque aperiam cumque est, quos quasi suscipit impedit adipisci
      molestiae ut tenetur porro eaque totam placeat? Libero molestias
      asperiores impedit odio fugiat, ipsum voluptate harum voluptatem suscipit
      inventore cupiditate sequi quibusdam quae est nisi nulla mollitia dolor
      incidunt sint voluptas adipisci voluptates exercitationem. Nostrum, soluta
      maiores, non nulla dolorum veniam sed veritatis ab ea repellat at pariatur
      cum porro quam? Est quia, mollitia impedit optio commodi incidunt quam
      ipsum doloribus numquam amet doloremque expedita aliquid provident
      consequatur. Error non optio molestias explicabo magni eligendi sequi
      praesentium. Ipsa a tenetur ab corrupti aliquid odio, amet id minima hic
      vero, iste exercitationem. Recusandae impedit labore soluta, eum at et
      reprehenderit! Ad, exercitationem veritatis. Officia fugiat, architecto
      beatae, nisi ullam voluptate ducimus corporis, illo fuga nam tempora fugit
      deserunt maiores sequi debitis. Voluptatibus eum accusamus reprehenderit
      corrupti tempora at alias incidunt, quis deleniti eius suscipit vitae, ut
      illo quas perspiciatis corporis quo magnam earum inventore? Nostrum,
      ullam! Accusamus ut commodi excepturi iusto dolor numquam similique est
      eveniet aperiam impedit ad molestias, ex facilis repellat quam id
      obcaecati. Provident ea repudiandae accusantium vel, ratione omnis tempora
      iure, voluptatem laudantium, culpa doloribus et est qui? Eum, sint
      obcaecati. Ullam, quibusdam nobis laboriosam alias quas distinctio
      provident fuga pariatur, sed beatae consequatur. A, quibusdam, cumque
      veniam molestias earum at, sit dolor doloribus reprehenderit accusantium
      consectetur animi dignissimos nobis! Voluptatem odit rerum sequi
      asperiores veniam! Unde, nisi? Numquam facilis unde aperiam consequuntur
      magnam nam voluptatibus provident quia, corporis perferendis quibusdam
      laudantium excepturi expedita maiores exercitationem nemo ea placeat
      asperiores inventore repellat recusandae nesciunt sequi odio? Cupiditate
      ducimus a doloribus voluptas perspiciatis inventore, libero iste, fugiat
      ad enim incidunt vero dolorum voluptate aperiam ea. Temporibus soluta
      laborum molestias minima deserunt culpa! Hic sapiente fugiat eum suscipit
      amet velit autem obcaecati distinctio. Veritatis iusto commodi repudiandae
      placeat dolor nemo, beatae explicabo aut ratione aliquam atque ut
      quibusdam, labore ipsa ea nesciunt fugiat porro exercitationem ipsam.
      Harum ex a perspiciatis eveniet incidunt necessitatibus molestiae
      excepturi, ratione dolores, ab repellendus odio, at vitae sed possimus non
      tempora hic? Labore, officiis!
    </p>
  </div>
</template>

<style scoped>
img {
  width: 45px;
  border: 2px solid;
  border-radius: 20%;
  background-color: hsl(0, 0%, 25%);
}
.con1 {
  /* outline: 1px solid red; */
  position: relative;
  /* left: -200px; */
  width: 600px;
  float: right;
  padding: 15px;
}
.con2 {
  /* outline: 1px solid blue; */
  display: flexbox;
  columns: 5 100px;
  column-rule: 3px solid;
}
</style>
