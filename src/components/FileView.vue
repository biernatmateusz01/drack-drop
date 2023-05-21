<template>
  <div class="w-screen hidden xl:flex h-screen">
    <div
      class="w-1/2 flex gap-3 p-4 flex-col"
      @drop.prevent="dropHandler($event, 'left')"
      @dragover.prevent
    >
      <span> Do zrobienia </span>
      <ul class="flex gap-3">
        <li
          v-for="person in persons"
          :key="person.id"
          class="border border-blue-500 hover:bg-blue-500 rounded-lg flex overflow-hidden flex-col w-48"
          draggable="true"
          @dragstart="dragStartHandler($event, person.id, 'left')"
        >
          <div class="w-full p-2">Dane:</div>
          <div class="h-2/3 p-2">
            <img
              src="https://cdn.pixabay.com/photo/2020/03/19/06/28/old-man-4946488__480.jpg"
              alt=""
            />
          </div>
          <div class="flex flex-col p-2">
            <span>{{ person.name }}</span>
            <span>{{ person.surname }}</span>
          </div>
        </li>
      </ul>
    </div>
    <div
      class="w-1/2 border-l border-blue-600 p-4 flex gap-3 flex-col"
      @drop.prevent="dropHandler($event, 'right')"
      @dragover.prevent
    >
      <span>Zrobione</span>
      <ul class="flex gap-3">
        <li
          v-for="person in empty"
          :key="person.id"
          class="flex flex-col p-2 rounded-xl border border-blue-600"
          draggable="true"
          @dragstart="dragStartHandler($event, person.id, 'right')"
        >
          <span>
            {{ person.name }}
          </span>
          <span>
            {{ person.surname }}
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
// import PersonFile from "../components/PersonFile.vue";
import { ref } from "vue";
interface Person {
  id: number;
  name: string;
  surname: string;
  age: number;
}

const persons = ref<Array<Person>>([
  {
    id: 1,
    name: "Józef",
    surname: "Kowalski",
    age: 14,
  },
  {
    id: 2,
    name: "Piotr",
    surname: "Kowalski",
    age: 14,
  },
  {
    id: 3,
    name: "Józef",
    surname: "Kowalski",
    age: 14,
  },
  {
    id: 4,
    name: "Piotr",
    surname: "Kowalski",
    age: 14,
  },
  {
    id: 5,
    name: "Józef",
    surname: "Kowalski",
    age: 14,
  },
  {
    id: 6,
    name: "Piotr",
    surname: "Kowalski",
    age: 14,
  },
]);

const empty = ref<Array<Person>>([]);

const dropHandler = (event: DragEvent, to: string) => {
  const variable = JSON.parse(event.dataTransfer?.getData("text/plain")!);

  if (variable.from === "left" && to === "right") {
    const element = persons.value.filter((el) => el.id === variable.id);
    empty.value.push(element[0]);

    const indexOfObject = persons.value.findIndex((object) => {
      return object.id === variable.id;
    });
    persons.value.splice(indexOfObject, 1);
  } else if (variable.from === "right" && to === "left") {
    const element = empty.value.filter((el) => el.id === variable.id);
    persons.value.push(element[0]);

    const indexOfObject = empty.value.findIndex((object) => {
      return object.id === variable.id;
    });
    empty.value.splice(indexOfObject, 1);
  }
};

const dragStartHandler = (event: DragEvent, id: number, from: string) => {
  const specialElement = {
    id: id,
    from: from,
  };
  event.dataTransfer?.setData("text/plain", JSON.stringify(specialElement));
};
</script>
