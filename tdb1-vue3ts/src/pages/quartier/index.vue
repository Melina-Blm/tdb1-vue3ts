<script setup lang="ts">
  import { supabase } from "../../supabase";
  import groupBy from "lodash/groupBy";
  import { Disclosure, DisclosureButton, DisclosurePanel } from "@headlessui/vue";


  const { data, error } = await supabase.from("quartiercommune").select("*");
  if (error) console.log("n'a pas pu charger la table quartiercommune :", error);
  </script>
  
  <template>
    <section class="flex flex-col">
      <h3 class="text-2xl">Liste des quartiers</h3>
     <ul>
        <li v-for="quartiercommune in (data as any[])">
          {{ quartiercommune.libelle_commune }} -
          {{ quartiercommune.libelle_quartier }}
        </li>
      </ul>
 
    
 
      <Disclosure v-for="(listeQuartier, libelle_commune) in groupBy(
      data,
      'libelle_commune'
    )"
    :key="libelle_commune">
    <DisclosureButton>{{libelle_commune}}</DisclosureButton>

    <DisclosurePanel>
      <li
  v-for="quartier in listeQuartier"
  :key="quartier.codeQuartier">
  <RouterLink
  :to="{
    name: 'quartier-id',
    params: { id: quartier.codeQuartier },
  }"
>{{ quartier.libelle_quartier }}</RouterLink>


</li>
    </DisclosurePanel>
      </Disclosure>


    </section>
  </template>