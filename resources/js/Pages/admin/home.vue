<template>
    <!-- <div class="home-back"> -->
    <div class="mt-3 mb-3 ml-3 ">
        <div>
        <v-btn @click="pdf">Download</v-btn>
    </div>
    </div>
    <div id="element-to-convert">

    <v-table class='mt-5'>
        <thead class='bg-grey text-subtitle-1 '>
            <tr>
                <th>#</th>
                <th>NIK</th>
                <th>NAMA</th>
                <th>PENGADUAN</th>
                <th>GAMBAR</th>
                <th>STATUS</th>
                <th>AKSI</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(row, i) in datas" :key="i++">
                <td>{{ i }}</td>
                <td>{{ row.nik }}</td>
                <td>{{ row.nama }}</td>
                <td>
                    <p v-html="row.isi" />
                </td>
                <td>
                    <v-img :src="'/storage/' + row.foto" style="width:50px" />
                </td>
                <td>{{ row.status }}</td>
                <td>
                    <v-menu>
                        <template v-slot:activator="{ props }">
                            <v-btn v-bind="props" icon="mdi mdi-pencil" round size="30" variant="text"></v-btn>
                        </template>
                        <v-list>
                            <v-list-item v-if="row.status == '0'" @click="openProses(row.id)">
                                <v-list-item-title>Proses</v-list-item-title>
                            </v-list-item>
                            <v-list-item v-else @click="openSelesai(row.id)">
                                <v-list-item-title>Selesai</v-list-item-title>
                            </v-list-item>
                        </v-list>
                    </v-menu>

                </td>
            </tr>
        </tbody>
    </v-table>
    </div>
    <v-dialog v-model="dialog_insert" width="700">
        <v-card>
            <v-card-text>
                <QuillEditor v-model:content="tanggapan" contentType="html" />
            </v-card-text>
            <v-card-actions>
                <v-btn v-show="!show" color="primary" @click="tanggapi">Tanggapi</v-btn>
                <v-btn v-show="show" color="primary" @click="selesai">Selesai</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
    <div style="display: none;">
        <table ref="table">
            <thead>
                <tr>
                    <th>#</th>
                    <th>NIK</th>
                    <th>NAMA</th>
                    <th>PENGADUAN</th>
                    <th>STATUS</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(row, i) in datas" :key="i++">
                    <td>{{ i }}</td>
                    <td>{{ row.nik }}</td>
                    <td>{{ row.nama }}</td>
                    <td>
                        <p v-html="row.isi" />
                    </td>
                    <td>{{ row.status }}</td>

                </tr>
            </tbody>
        </table>
    </div>
    <!-- </div> -->
</template>

<script>
import html2pdf from 'html2pdf.js'
import { router } from '@inertiajs/core'
import layoutVue from '../layout/layout.vue'
export default {
    layout: layoutVue,
    props: {
        datas: Array,
    },
    data() {
        return {
            id_pengaduan: "",
            tanggapan: "",
            dialog_insert: false,
            show: false,
            uri: 'data:application/vnd.ms-excel;base64,',
            template: '<html xmlns:o="urn:schemas-microsoft-com:office:office" xmlns:x="urn:schemas-microsoft-com:office:excel" xmlns="http://www.w3.org/TR/REC-html40"><head><!--[if gte mso 9]><xml><x:ExcelWorkbook><x:ExcelWorksheets><x:ExcelWorksheet><x:Name>{worksheet}</x:Name><x:WorksheetOptions><x:DisplayGridlines/></x:WorksheetOptions></x:ExcelWorksheet></x:ExcelWorksheets></x:ExcelWorkbook></xml><![endif]--></head><body><table>{table}</table></body></html>',
            base64: function (s) { return window.btoa(unescape(encodeURIComponent(s))) },
            format: function (s, c) { return s.replace(/{(\w+)}/g, function (m, p) { return c[p]; }) }
        }
    },
    methods: {
        openProses($id) {
            this.id_pengaduan = $id
            this.dialog_insert = true
            this.show = false
        },
        openSelesai($id) {
            this.id_pengaduan = $id
            this.dialog_insert = true
            this.show = true
        },
        tanggapi() {
            router.visit("/petugas/tanggapan/proses", {
                method: 'post',
                data: {
                    id_pengaduan: this.id_pengaduan,
                    tanggapan: this.tanggapan
                },
                preserveState: true,
                preserveScroll: true,
                onSuccess: () => {
                    this.dialog_insert = false
                    this.id_pengaduan = ""
                    this.tanggapan = ""
                }
            })
        },
        selesai() {
            router.visit("/petugas/tanggapan/selesai", {
                method: 'post',
                data: {
                    id_pengaduan: this.id_pengaduan,
                    tanggapan: this.tanggapan
                },
                preserveState: true,
                preserveScroll: true,
                onSuccess: () => {
                    this.dialog_insert = false
                    this.id_pengaduan = ""
                    this.tanggapan = ""
                }
            })
        },
        pdf() {
            html2pdf(document.getElementById ("element-to-convert"),{
                margin: 1,
                filename:"laporan.pdf"
            })
        },
    }
}
</script>
<style scoped>

</style>>

