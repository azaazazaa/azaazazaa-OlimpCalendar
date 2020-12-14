<template>
  <!--Overlay section start-->
    <div id="overlay-background" class="overlay" ref="overlayBackground" @click="close">
    
        <div class="overlay-content">

            <div class="form-header overlay-header">
                <h4>Создать мероприятие</h4>
                <a href="javascript:void(0)" class="closebtn" id="closebtn" ref="closebtn"
                  @click="close"
                >&times;</a>
            </div>

            <div class="form-row">
                <select name="place" id="place" @change="changeRooms" ref="place">
                    <option value="" disabled selected hidden>Площадка</option>
                    <option value="OLIMP">ОЛИМП, Кронверкский пр.49а</option>
                    <option value="SNLO">СНЛО, пер. Гривцова 14</option>
                </select>
            </div>

            <div class="form-row">
                <select name="room" id="room" disabled ref="room">
                    <option value="" disabled selected hidden>Аудитория</option>
                    <!-- Populated with JavaScript -->
                </select>
            </div>
            
            <div class="form-row">
                <select name="activityType" id="activityType" ref="activityType">
                    <option value="" disabled selected hidden>Тип мероприятия</option>
                    <option value="Workshop">Мастерская</option>
                    <option value="ProjectGroup">Проектная группа</option>
                    <option value="Lectorium">Лекторий</option>
                    <option value="Club">Клуб</option>
                </select>
            </div>

            <div class="form-row input-label">
                <label for="startingTime">Время проведения</label>
            </div>
            <div class="time-wrap">
                <input type="time" name="startingTime" id="startingTime" ref="startTime">
                <h4>&ndash;</h4>
                <input type="time" name="endingTime" id="endingTime" ref="endTime">
            </div>
            <!--
            <div class="form-row input-label">
                <label for="actvityName">Название мероприятия</label>
            </div>
            -->
            <div class="form-row">
                <div class="textareaContainer">
                    <div class="textarea" contenteditable="true" placeholder="Название мероприятия" ref="name">Название мероприятия</div>
                </div>
            </div>

            <div class="form-row organizer">
                <select name="organizer" id="organizer" ref="organizer">
                    <option value="" disabled selected hidden>Руководитель</option>
                    <option value="KBodrov">Бодров Кирилл</option>
                    <option value="ARodionova">Родионова Алиса</option>
                    <option value="NTolstoba">Толстоба Надежда</option>
                    <option value="SZablotskaya">Заблоцкая Светлана</option>
                </select>
                <i class="fas fa-user-circle" aria-hidden="true"></i>
            </div>

            <div class="submit-btn">
                <input type="submit" value="Сохранить" @click="onSubmit">
            </div>
            <div class="warning margin-horiz hidden" ref="warn">Все поля должны быть заполнены</div>
        </div>
    </div>
    
    <!--Overlay section end-->
</template>

<script>
export default {
  props:{
    date: {
      type: Array,
    },
  },
  data(){
    return{
      inputIsAble: true,
      data: JSON.parse(`{ 
          "id": [1, 2],
          "location": [   
              "ОЛИМП, Кронверкский пр.49",
              "СНЛО, пер. Гривцова 14"
          ],
          "place": [
              {"auditoria": [
                  "Коворкинг (ауд.511)",
                  "Лекционная (ауд.501)",
                  "Мастерская (ауд.512)"
                  ]
              },
              {"auditoria": [
                  "Основная (ауд. 419)",
                  "Новая (ауд. 342)",
                  "Подвал (ауд. 005)"
                  ]
              }
          ]}`
      ),
      selectedData: [
        {
          place: '',
          classroom: '',
          curator: '',
          date: `${this.date[0]}.${ this.date[1] + 1 }.${this.date[0]}`,
          activityType: '',
          name: '',
        },

      ]
    }
  },
  methods:{
    close(event){
      let evt = event.target;
      if (evt.classList.contains("overlay") || evt.classList.contains("closebtn")){
        this.inputIsAble = false;
        this.$refs.warn.classList.add('hidden')
      };
     },
     changeRooms(e){
      if (e.target.value !== "") {
        // alert(e.target.value);
        this.$refs.room.disabled = false;

        switch (e.target.value) {
          case "OLIMP":
            this.clearSelect(this.$refs.room);
            this.populateSelect(this.data.place[0].auditoria);
            break;
          case "SNLO":
            this.clearSelect(this.$refs.room);
            this.populateSelect(this.data.place[1].auditoria);
            break;
          default:
            text = "Error, Sir!";
          }
      }
      else this.$refs.room.disabled = true;
     },
     populateSelect(target) {
      for (let i = 0; i < target.length; i++){
        let option = document.createElement("option");
        option.value = target[i];
        option.text = target[i];
        this.$refs.room.appendChild(option);
      }
    },
    clearSelect(target) {
      while (target.firstChild) {
        target.removeChild(target.lastChild);
      };
      let option = document.createElement("option");
      option.value = "placeholder";
      option.text = "Аудитория";
      option.disabled = true;
      option.selected = true;
      option.hidden = true;
      this.$refs.room.appendChild(option);
    },
    onSubmit(e){
      if (!(this.$refs.place.value || this.$refs.room.value || this.$refs.organizer.value || this.$refs.activityType.value || this.$refs.name.textContent)){
        e.preventDefault();
        this.selectedData.place = this.$refs.place.value;
        this.selectedData.classroom = this.$refs.room.value;
        this.selectedData.curator = this.$refs.organizer.value;
        this.selectedData.activityType = this.$refs.activityType.value;
        this.selectedData.name = this.$refs.name.textContent;
        console.log(this.selectedData);
      } else {
        this.$refs.warn.classList.remove('hidden')
      }
    }
  },
  computed: {
     
   },
   watch: {
    date: function (val) {
      this.$refs.overlayBackground.style.height = "100%";
      this.inputIsAble = true;
    },
    inputIsAble: function(val) {
      if (!val){
        this.$refs.overlayBackground.style.height = "0%";
      }
    },
    
   }
}
</script>
