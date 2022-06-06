<template>
  <div>
    <div class="row p-4" v-if="tab==1">
      <nav style="--bs-breadcrumb-divider: '>'; padding-left: 0px;" >
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><a style="color: #323232; opacity: 0.64;font-family: 'Open Sans'; font-style: normal; font-weight: 400; line-height: 24px; letter-spacing: 0.1px;">Inicio</a></li>
        <li class="breadcrumb-item"><a style="color: #4FB9BB;letter-spacing: 0.1px;font-style: normal; font-family: 'Open Sans'; font-weight: 400; line-height: 24px;">Administradores</a></li>
      </ol>

      <h3 style="font-family: 'Montserrat'; font-style: normal; font-weight: 600; font-size: 20px; line-height: 28px; letter-spacing: -0.05em; color: #323232; width: 1087px; height: 28px;"> Administradores de la consola </h3 >
      </nav>
      <div class="mt-4 mb-4">
      <div class="row">
        <div class="col-6">
          <div class="row">
            <div class="col-6">
              <input type="text" class="form-control" placeholder="Buscar" aria-label="share" aria-describedby="basic-addon1" style=" background: #E8EAF0; opacity: 0.24; border-radius: 32px; ">
            </div>
            <div class="col-6">
              <div class="row">
                <div class="col-6">
                  <button type="button" class="btn btn-light" style="color: #323232;">Buscar</button>
                  <button type="button" class="btn ml-3" style="color: white; background: #4FB9BB;"><i class="fa-solid fa-filter"></i></button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-6">
          <button type="button" @click="changeAdd" style="float: right; background-color: #4FB9BB; border-color: #4FB9BB;" class="btn btn-primary ml-3">Agregar nuevo admin</button>
          <button type="button" style="float: right; color:#4FB9BB; border-color:#4FB9BB" class="btn btn-outline-primary" >Descargar</button>
        </div>
      </div>
      </div>
      <div class="col-lg-12">
      <div>
        <div>
          <v-data-table
            show-select
            single-select
            :headers="headers"
            :items="desserts"
            :items-per-page="5"
            class="elevation-1"
          >
          <template v-slot:[`item.status`]="{ item }">
            <span :class="item.status == 'Activo' ? 'badge rounded-pill bg-success' : 'badge rounded-pill bg-danger' ">{{item.status}}</span>
          </template>
          <template v-slot:[`item.details`]="{ item }">
            <div class="row" style="width: 50%;">
              <div class="col-4">
                <a @click="editItem(item)">
                  <i class="fa-regular fa-pen-to-square" style="color:#4FB9BB;"></i>
                </a>
              </div>
              <div class="col-4">
                <a @click="deleteItem(item)">
                  <i class="fa-regular fa-trash-can" style="color:#EB5757;"></i>
                </a>
              </div>
              <div class="col-4">
                <a @click="showItem(item)">
                  <i class="fa-regular fa-eye" style="color:#323232;"></i> 
                </a>
              </div>
            </div>
          </template>
          </v-data-table>
        </div>
      </div>
      </div>
      <v-dialog
        v-model="dialog"
        max-width="500"
      >
      <v-card>
          <v-card-title class="text-h5 white lighten-2">
            <p style="color:#4FB9BB;"> Eliminar Administrador</p>
          </v-card-title>

          <v-card-text>
            <h5>¿Estas seguro de eliminar a este administrador?  </h5> 
            <div class="mt-5">
              <div class="row">
                <div class="col-2">
                    <img src="../assets/redonda.png" class="imgRedonda"/>
                </div>
                <div class="col-10">
                  {{current_item.name}} <br>
                  {{current_item.email}}
                </div>
              </div>
              <p>
              </p>
              <p>
              </p>
            </div>
          </v-card-text>

          <v-divider></v-divider>

            <v-spacer></v-spacer>
            <div class="row" style="padding: 2% 17% 2% 17%;">
              <div class="col-6">
                  <button  @click="dialog = false" type="button" class="btn btn-light" style="color: #323232;">Cancelar</button>
              </div>
              <div class="col-6">
                  <button  @click="dialog = false" type="button" class="btn ml-3" style="color: white; background: #4FB9BB;"> Si, eliminar</button>
              </div>
            </div>                     
          <v-card-actions>
          </v-card-actions>
      </v-card>
      </v-dialog>
    </div>
    <div class="row p-4" v-if="tab==2">
      <nav style="--bs-breadcrumb-divider: '>'; padding-left: 0px;" >
        <ol class="breadcrumb">
          <li class="breadcrumb-item"><a style="color: #323232; opacity: 0.64;font-family: 'Open Sans'; font-style: normal; font-weight: 400; line-height: 24px; letter-spacing: 0.1px;">Inicio</a></li>
          <li class="breadcrumb-item"><a style="color: #4FB9BB;letter-spacing: 0.1px;font-style: normal; font-family: 'Open Sans'; font-weight: 400; line-height: 24px;">Administradores</a></li>
        </ol>

        <h3 class="p-4" style="font-family: 'Montserrat'; font-style: normal; font-weight: 600; font-size: 20px; line-height: 28px; letter-spacing: -0.05em; color: #323232; width: 1087px; height: 28px;"> Editar Administrador </h3 >
        </nav>
        <div class="row p-4">
        <div class="col-4 pb-5">
            <v-file-input
            :rules="rules"
            accept="image/png, image/jpeg, image/bmp"
            placeholder="Pick an avatar"
            prepend-icon="mdi-camera"
            label="Avatar"
          ></v-file-input>
            <v-text-field
              label="Nombres"
              v-model="current_item.name"
            ></v-text-field>
            <v-text-field 
              label="Apellidos"
              v-model="current_item.surname">
            </v-text-field>
            <v-text-field 
              label="Email"
              v-model="current_item.email">
            </v-text-field>
            <v-select
              :items="itemsAreas"
              label="Tipo de area"
              v-model="current_item.area"
            ></v-select>
            <v-select
              :items="itemsStatus"
              label="Tipo de area"
              v-model="current_item.status"
            ></v-select>
        </div>
        <hr>
        <div class="row">
          <div class="col-6">
            <button type="button" @click="returnAdmin" style=" color:#4FB9BB; border-color:#4FB9BB" class="btn btn-outline-primary mr-3" > Cancelar </button>
            <button type="button"  @click="successView" style=" background-color: #4FB9BB; border-color: #4FB9BB;" class="btn btn-primary mr-3"> Guardar administrador</button>
          </div>
        </div>

        </div>
      </div>
      <div class="row p-4" v-if="tab==3">
          <nav style="--bs-breadcrumb-divider: '>'; padding-left: 0px;" >
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><a style="color: #323232; opacity: 0.64;font-family: 'Open Sans'; font-style: normal; font-weight: 400; line-height: 24px; letter-spacing: 0.1px;">Inicio</a></li>
              <li class="breadcrumb-item"><a style="color: #4FB9BB;letter-spacing: 0.1px;font-style: normal; font-family: 'Open Sans'; font-weight: 400; line-height: 24px;">Administradores</a></li>
            </ol>
          <h3 class="p-4" style="font-family: 'Montserrat'; font-style: normal; font-weight: 600; font-size: 20px; line-height: 28px; letter-spacing: -0.05em; color: #323232; width: 1087px; height: 28px;"> Agregar Administrador </h3 >
          </nav>
        <div class="row p-4">
        <div class="col-4 pb-5">
            <v-file-input
            :rules="rules"
            accept="image/png, image/jpeg, image/bmp"
            placeholder="Pick an avatar"
            prepend-icon="mdi-camera"
            label="Avatar"
          ></v-file-input>
            <v-text-field
              label="Nombres"
              v-model="new_item.name"
            ></v-text-field>
            <v-text-field 
              label="Apellidos"
              v-model="new_item.surname">
            </v-text-field>
            <v-text-field 
              label="Email"
              v-model="new_item.email">
            </v-text-field>
            <v-select
              :items="itemsAreas"
              label="Tipo de area"
              v-model="new_item.area"
            ></v-select>
            <v-select
              :items="itemsStatus"
              label="Tipo de area"
              v-model="new_item.status"
            ></v-select>
        </div>
        <hr>
        <div class="row">
          <div class="col-6">
            <button type="button" @click="returnAdmin" style=" color:#4FB9BB; border-color:#4FB9BB" class="btn btn-outline-primary mr-3" > Cancelar </button>
            <button type="button" @click="successView" style=" background-color: #4FB9BB; border-color: #4FB9BB;" class="btn btn-primary mr-3"> Guardar administrador</button>
          </div>
        </div>

        </div>
      </div>
        <div class="row p-4" v-if="tab==4">
          <nav style="--bs-breadcrumb-divider: '>'; padding-left: 0px;" >
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><a style="color: #323232; opacity: 0.64;font-family: 'Open Sans'; font-style: normal; font-weight: 400; line-height: 24px; letter-spacing: 0.1px;">Inicio</a></li>
              <li class="breadcrumb-item"><a style="color: #4FB9BB;letter-spacing: 0.1px;font-style: normal; font-family: 'Open Sans'; font-weight: 400; line-height: 24px;">Administradores</a></li>
            </ol>
          <h3 class="p-4" style="font-family: 'Montserrat'; font-style: normal; font-weight: 600; font-size: 20px; line-height: 28px; letter-spacing: -0.05em; color: #323232; width: 1087px; height: 28px;"> Administradores de consola </h3 >
          </nav>
        <div class="row p-4">
        <div class="col-4 pb-5">
          <i class="fa-solid fa-circle-check" style="font-size: 70px; color:#4FB9BB;"></i>
          <h3 class="mt-2">
            Cambios guardados.
          </h3>
          <p class="mt-3">
            Tu información ha sido guardada con éxito.
          </p>
        </div>
        <hr>
        <div class="row">
          <div class="col-6">
            <button @click="returnAdmin" type="button" style=" background-color: #4FB9BB; border-color: #4FB9BB;" class="btn btn-primary mr-3"> Regresar a mis administradores</button>
          </div>
        </div>

        </div>
      </div>
      <div class="row p-4" v-if="tab==5">
      <nav style="--bs-breadcrumb-divider: '>'; padding-left: 0px;" >
        <ol class="breadcrumb">
          <li class="breadcrumb-item"><a style="color: #323232; opacity: 0.64;font-family: 'Open Sans'; font-style: normal; font-weight: 400; line-height: 24px; letter-spacing: 0.1px;">Inicio</a></li>
          <li class="breadcrumb-item"><a style="color: #4FB9BB;letter-spacing: 0.1px;font-style: normal; font-family: 'Open Sans'; font-weight: 400; line-height: 24px;">Administradores</a></li>
        </ol>

        <h3 class="p-4" style="font-family: 'Montserrat'; font-style: normal; font-weight: 600; font-size: 20px; line-height: 28px; letter-spacing: -0.05em; color: #323232; width: 1087px; height: 28px;"> Detalles Administrador </h3 >
        </nav>
        <div class="row p-4">
          <div class="row">
            <div class="col-4">
              <a> Nombres</a> 
              <h5>{{current_item.name ? current_item.name : 'No tiene'}}</h5>
            </div>
            <div class="col-4">
              <a> Apellidos</a> 
              <h5>{{current_item.surname ? current_item.surname : 'No tiene'}}</h5>
            </div>
              <div class="col-4">
              <a> Correo</a> 
              <h5>{{current_item.email ? current_item.email : 'No tiene' }}</h5>
            </div>
          </div>
             <div class="row">
            <div class="col-6">
              <a> Area</a> 
              <h5>{{current_item.area ? current_item.area : 'No tiene' }}</h5>
            </div>
            <div class="col-6">
              <a> Estatus</a> 
              <h5>{{current_item.status ? current_item.status : 'No tiene'}}</h5>
            </div>
          </div>
        <hr>
        <div class="row">
          <div class="col-6 mt-4">
            <button type="button"  @click="editItem(current_item)" style=" background-color: #4FB9BB; border-color: #4FB9BB;" class="btn btn-primary mr-3"> Editar administrador</button>
          </div>
        </div>

        </div>
      </div>
  </div>

</template>

<script>
  export default {
    data () {
      return {
        rules: [
          value => !value || value.size < 2000000 || 'Avatar size should be less than 2 MB!',
        ],
        tab:1,
        dialog: false,
        current_item: {},
        new_item: {},
        itemsAreas: ['Recursos Humanos', 'Desarollo', 'Otra',],
        itemsStatus: ['Activo', 'Inactivo',],
        headers: [
          {
            text: 'Administradores',
            align: 'start',
            sortable: true,
            value: 'name',
          },
          { text: 'Area', value: 'area', sortable: true },
          { text: 'Correo', value: 'email', sortable: true },
          { text: 'Estatus', value: 'status', sortable: true },
          { text: 'Detalles', value: 'details', sortable: false },
        ],
        desserts: [
          {
            name: 'Gustavo korsgaard',
            area: 'Recursos Humanos',
            email: 'correo@gmail.com',
            status: 'Activo',
          },
          {
            name: 'Maria Dias',
            area: 'Recursos Humanos',
            email: 'correo@gmail.com',
            status: 'Activo',
          },
          {
            name: 'Maren staton',
            area: 'Recursos Humanos',
            email: 'correo@gmail.com',
            status: 'Activo',
          },
          {
            name: 'Paityn ekstro,',
            area: 'Recursos Humanos',
            email: 'correo@gmail.com',
            status: 'Inactivo',
          },
          {
            name: 'Kierra bergson',
            area: 'Recursos Humanos',
            email: 'correo@gmail.com',
            status: 'Activo',
          },
          {
            name: 'Talan rosser',
            area: 'Recursos Humanos',
            email: 'correo@gmail.com',
            status: 'Activo',
          },
        ],
      }
    },
    methods: {
      editItem(item) {
        this.tab = 2;
        this.current_item = item;
      },
      changeAdd() {
        this.tab = 3;
      },
      returnAdmin() {
        this.tab = 1;
      },
      showItem(item) {
        this.tab = 5;
        this.current_item = item;
      },
      deleteItem(item) {
        this.current_item = item; 
        this.dialog = true;     
      },
      successView() {
        this.tab = 4;
      },
    },
  }
</script>