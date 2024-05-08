<script setup>
// vue3 컴포지션 api 가져오기
import { reactive, computed } from 'vue';
// data객체를 reactive함수를 통해 반응성 주입하여 초기화
const data = reactive({
  newItem: '',
  items: [],
});

//할일추가: data.newItem은 입력필드와 연결되있으므로 빈값이 아닐경우 추가
// 추가된 데이터 마다 고유 id 추가하여 데이터 구분 가능하도록 해줌
// 할일추가후 입력필드 빈칸으로 초기화
function addItem() {
  if (data.newItem !== '') {
    data.items.push({
      id: data.items.length + 1,
      text: data.newItem,
      completed: false,
    });
    data.newItem = '';
    // console.log(data.items);
  }
}

// computed함수: data.items가 변경될때마다 매번 함수 실행(계산결과를 캐싱하여 성능 최적화)
// 할일갯수
const totalItems = computed(() => data.items.length);

// 완료갯수
const isComplete = computed(
  () => data.items.filter((item) => item.completed).length
);

function deleteItem(id) {
  // console.log(id);
  // id가 일치하는 요소 찾기
  // const itemToDelete = data.items.find((item) => item.id === id);
  // console.log(itemToDelete);
  // 삭제할 아이템 위치 찾기
  // const index = data.items.indexOf(itemToDelete);
  // console.log(index);

  //splice(시작위치, 삭제갯수)로 삭제하기
  const index = data.items.findIndex((item) => item.id === id);
  console.log(index);
  // data.items.splice(index, 1);
}
</script>

<template>
  <main class="app">
    <h1>Simple to-do list</h1>
    <div class="todo_count">
      완료: {{ isComplete }} / 할 일: {{ totalItems }}
    </div>
    <div class="todo_add">
      <!-- v-model 디렉티브는 폼요소와 데이터를 양방향으로 연결 -->
      <!-- v-on:이벤트명으로 이벤트 연결 -->
      <input
        type="text"
        placeholder="할 일을 입력하세요"
        title="할 일을 입력하세요"
        v-model="data.newItem"
        v-on:keyup.enter="addItem()"
      />
      <button type="button" class="add_btn" v-on:click="addItem()">Add</button>
    </div>
    <ul class="todo_list">
      <!-- v-bind 디렉티브는 데이터를 단방향으로 연결하여 가져오기만함 -->
      <!-- 리스트는 고유 id를 key속성에 연결하여 리스트의 순서가 바뀌어도 구별가능하게함 -->
      <li
        v-for="item in data.items"
        v-bind:key="item.id"
        v-bind:class="{ completed: item.completed }"
      >
        <!-- 라벨클릭시 for로 연결된 체크박스가 클릭되어 true, false가 발생되며
        v-model="item.completed"에 의해 completed속성값이 true, false가됨 -->
        <input
          v-bind:id="`check${item.id}`"
          type="checkbox"
          v-model="item.completed"
        />
        <label v-bind:for="`check${item.id}`">{{ item.text }}</label>
        <button
          type="button"
          v-on:click="deleteItem(item.id)"
          class="remove_btn"
        >
          Remove
        </button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.app {
  padding: 40px;
}
.app h1 {
  font-size: 30px;
  color: #000;
  font-weight: 700;
  margin-bottom: 20px;
}
.todo_count {
  margin: 10px 0;
}
.todo_add {
  display: flex;
}

.todo_add input[type='text'] {
  height: 40px;
  padding: 0 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: calc(100% - 59px);
}
.todo_add .add_btn {
  height: 40px;
  padding: 0 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: #fff;
  background: #333;
  border: none;
}

.todo_list {
  margin-top: 20px;
}
.todo_list li {
  display: flex;
  margin-bottom: 10px;
}
.todo_list label {
  flex-grow: 1;
}
.todo_list li.completed label {
  color: #ccc;
  text-decoration: line-through;
}
.todo_list .remove_btn {
  height: 32px;
  padding: 0 5px;
  background: none;
  border: 1px solid #b83030;
  color: #b83030;
  border-radius: 4px;
  margin-left: 20px;
  flex-shrink: 0;
}
</style>
