<template>
    <div>
        <div class="accordion" id="accordionExample">
            <div class="accordion-item" v-for="(item, index) in folder_structure" :key="index+'p1'">
                <h2 class="accordion-header" :id="'heading'+item.name">
                    <button class="accordion-button" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapse'+item.name" aria-expanded="true" :aria-controls="'collapse'+item.name">
                        {{ item.name }}
                    </button>
                </h2>
                <div :id="'collapse'+item.name" class="accordion-collapse collapse show" :aria-labelledby="'heading'+item.name" data-bs-parent="#accordionExample">
                    <div class="accordion-body">
                        <div class="accordion" :id="'accordionExample'+item.name">
                            <div class="accordion-item" v-for="(item, index) in item.children" :key="index+'p2'">
                                <h2 class="accordion-header" :id="'heading'+item.name">
                                    <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapse'+item.name" aria-expanded="true" :aria-controls="'collapse'+item.name">
                                        {{ item.name }}
                                    </button>
                                </h2>
                                <div :id="'collapse'+item.name" class="accordion-collapse collapse" :aria-labelledby="'heading'+item.name" :data-bs-parent="'#accordionExample'+item.name">
                                    <div class="accordion-body">
                                        <div class="accordion" :id="'accordionExample'+item.name">
                                            <div class="accordion-item" v-for="(item, index) in item.children" :key="index+'p3'">
                                                <h2 class="accordion-header" :id="'heading'+item.name">
                                                    <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapse'+item.name" aria-expanded="true" :aria-controls="'collapse'+item.name">
                                                        {{ item.name }}
                                                    </button>
                                                </h2>
                                                <div :id="'collapse'+item.name" class="accordion-collapse collapse" :aria-labelledby="'heading'+item.name" :data-bs-parent="'#accordionExample'+item.name">
                                                    <div class="accordion-body">
                                                        <div class="form-check float-end">
                                                            <input class="form-check-input" v-model="item.checked" type="checkbox" value="" :id="'flexCheckDefault'+item.name" @change="selectAll(item.name, item.checked)">
                                                            <label class="form-check-label" :for="'flexCheckDefault'+item.name">
                                                                Select all
                                                            </label>
                                                        </div>
                                                        <div class="form-check" v-for="(file, index) in item.children" :key="index+'c'">
                                                            <input class="form-check-input" v-model="file.checked" type="checkbox" value="" :id="'flexCheckDefault'+file.name" @change="removeSelectedFolders()">
                                                            <label class="form-check-label" :for="'flexCheckDefault'+file.name">
                                                                {{ file.name }} ({{ file.size }})
                                                            </label>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'HomePage',
  props: ['folder_structure'],
  methods: {
    selectAll(name, status) {
        this.folder_structure.forEach(element => {
            element.children.forEach(element => {
                element.children.forEach(element => {
                    element.checked=false;
                    if(element.name == name && status == true){
                        element.checked=true;
                        element.children.forEach(element => {
                            element.checked=true;
                        });
                        name = '';
                    } else {
                        element.children.forEach(element => {
                            element.checked=false;
                        });
                    } 
                });
            });
        });
    },
    removeSelectedFolders() {
        this.folder_structure.forEach(element => {
            element.children.forEach(element => {
                element.children.forEach(element => {
                    element.checked=false;
                });
            });
        });
    },
  }
}
</script>