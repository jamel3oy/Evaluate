<template>
    <div class="grid">
        <div class="col-12 lg:col-12 xl:col-12">
            <div class="col md:col-12 text-right">
                <Button label="Export" icon="pi pi-file-word" class="mr-2 mb-2 " @click="exportToWord" ></Button>
            </div>
            <div class="card mb-0">
                <div class="formgroup-inline mb-1">
                    <div class="col md:col-3">
                        <h3 class="mb-4 card-header">
                            <i class="pi pi-fw pi-folder-open" style="font-size: x-large;"></i> แบบ ป01
                        </h3>
                    </div>
                    <div class="col md:col-3 text-left">
                        <label for="dropdownProportion">สัดส่วน :</label>
                        <Dropdown id="dropdownProportion" v-model="dropdownProportion" :options="dropdownProportions" optionLabel="name" placeholder="เลือกสัดส่วน"></Dropdown>
                    </div>
                    <div class="col md:col-3 text-left">
                        <label for="dropdownItemYear">ปีงบประมาณ :</label>
                        <Dropdown id="dropdownItemYear" v-model="dropdownItemYear" :options="dropdownItemsYear" optionLabel="name" placeholder="เลือกปีงบประมาณ"></Dropdown>
                    </div>
                    <div class="col md:col-3 text-right">
                        <Button icon="pi pi-search" severity="help" class="mb-2 mr-2" label="เลือกข้อมูลแบบประเมิน ป.01" @click="OpenDialogP01" /> 
                        <Button icon="pi pi-plus" severity="info" class="mb-2 mr-2" label="เพิ่มข้อมูลแบบประเมิน" @click="OpenDialogAdd" /> 
                    </div> 
                </div>
                <!-- แสดงข้อมูลบันทึก -->
                <table class="table">
                    <thead>
                        <tr>
                            <th rowspan="2" style="width: 20%;">(1) <br> กิจกรรม / โครงการ / งาน</th>
                            <th rowspan="2" style="width: 24%;">
                            (2) <br> ตัวชี้วัด / เกณฑ์การประเมิน <br>
                            (1.ถูกต้อง 2.ครบถ้วน <br> 3.ตรงเวลา)
                            </th>
                            <th colspan="5" style="width: 15%;">(3) <br> ระดับค่าเป้าหมาย</th>
                            <th rowspan="2" style="width: 7%;">(4) <br> ค่าคะแนน ที่ได้</th>
                            <th rowspan="2" style="width: 10%;">(5) <br> น้ำหนัก (ความสำคัญ/ <br> ความยากง่ายของงาน)</th>
                            <th rowspan="2" style="width: 10%;">(6) <br> ค่าคะแนน <br>ถ่วงน้ำหนัก <br> (4) X (5) 100</th>
                            <th rowspan="2" style="width: 9%;"><br> ตัวเลือก <br></th>
                        </tr>
                        <tr>
                            <th>1</th>  
                            <th>2</th>  
                            <th>3</th>  
                            <th>4</th>  
                            <th>5</th>  
                        </tr>
                    </thead>
                    <tbody>
                        <template v-for="(h, ind) in products_personX" :key="ind">  
                            <tr>
                                <td style="text-align: left;" colspan="9">
                                    <b style="color: blue;">{{ h.id }}. {{ h.nameH }}</b>
                                </td>
                                <td class="text-center" style="color: blue;"><b>{{ h.h_weight }}%</b></td> 
                                <td></td>
                            </tr> 
                            <tr v-for="(subP01, idx) in h.subP01sX" :key="idx" style="vertical-align: baseline;"> 
                                <td style="text-align: left;">{{ subP01.p01_no }} {{ subP01.p01_subject }}</td>
                                <td style="text-align: left;">
                                    <b>ตัวชี้วัดที่ {{ idx+1 }} {{ subP01.p01_subject }}</b>
                                    <p v-for="(subIitem, idI) in subP01.subITems" :key="idI" style="padding-left: 8px;margin-bottom: 5px;">
                                        <div v-if="subIitem.ind_no!=0"><b>ระดับ {{ subIitem.ind_no }}</b> {{ subIitem.ind_Items }}</div>
                                        <div v-if="subIitem.ind_no==0"><b>{{ subIitem.ind_Items }}</b></div>
                                    </p>
                                </td>
                                <td style=" vertical-align: middle;" class="text-center">
                                    <b v-if="subP01.p01_target==1">&#10003;</b> 
                                    <b v-if="subP01.p01_target!=1"></b> 
                                </td>
                                <td style=" vertical-align: middle;" class="text-center"> 
                                    <b v-if="subP01.p01_target==2">&#10003;</b> 
                                    <b v-if="subP01.p01_target!=2"></b> 
                                </td>
                                <td style=" vertical-align: middle;" class="text-center">
                                    <b v-if="subP01.p01_target==3">&#10003;</b> 
                                    <b v-if="subP01.p01_target!=3"></b> 
                                </td>
                                <td style=" vertical-align: middle;" class="text-center"> 
                                    <b v-if="subP01.p01_target==4">&#10003;</b> 
                                    <b v-if="subP01.p01_target!=4"></b> 
                                </td>  
                                <td style=" vertical-align: middle;" class="text-center"> 
                                    <b v-if="subP01.p01_target==5">&#10003;</b> 
                                    <b v-if="subP01.p01_target!=5"></b> 
                                </td>
                                <td style=" vertical-align: middle;" class="text-center">{{ subP01.p01_score }}</td>
                                <td style=" vertical-align: middle;" class="text-center">{{ subP01.p01_weight }}%</td> 
                                <td style=" vertical-align: middle;" class="text-center">{{ (subP01.p01_score * subP01.p01_weight / 100).toFixed(2) }}</td>
                                
                                <td style=" vertical-align: middle;" class="text-center">
                                    <SplitButton label="เลือก" :model="itemsBtu(subP01)" severity="warning" class="mb-2 mr-2"></SplitButton>
                                </td> 
                            </tr>
                        </template>
                        <tr>
                            <td style="text-align: right" colspan="8">
                                <b style="color: blue;">(7) ผลรวม</b>
                            </td>
                            <td class="text-center" style="color: blue;"><b>15%</b></td> 
                            <td></td>
                            <td></td>
                        </tr>
                        <tr>
                            <td style="text-align: right; vertical-align: middle;" colspan="7">
                                <b style="color: blue;">(8) สรุปคะแนนส่วนผลสัมฤทธิ์ของงาน = </b>
                            </td>
                            <td colspan="2" class="text-center" style="vertical-align: middle;line-height: 1.5;">
                                <b style="color: blue;">ผลรวมของค่าคะแนนถ่วงน้ำหนัก</b>
                                <hr style="border: 1px solid black; width: 100%; ">
                                <b style="color: blue;">จำนวนระดับค่าเป้าหมาย = 5 </b>
                            </td>
                            <td></td>
                            <td></td>
                        </tr>  
                    </tbody>
                </table>   
            </div> 
        </div> 

        <!-- เลือกข้อมูลแบบประเมิน ป.01 -->
        <Dialog header="จัดการแบบ ป01" maximizable v-model:visible="DialogAddP01" :breakpoints="{ '960px': '75vw' }" :style="{ width: '90vw' }" :modal="true" position="top">
            <form>
                <div class="p-fluid formgrid grid">
                    <div class="field col-12 md:col-12">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th rowspan="2" style="width: 9%;"><br> ตัวเลือก <br></th>
                                    <th rowspan="2" style="width: 20%;">(1) <br> กิจกรรม / โครงการ / งาน</th>
                                    <th rowspan="2" style="width: 24%;">
                                        (2) <br> ตัวชี้วัด / เกณฑ์การประเมิน <br>
                                        (1.ถูกต้อง 2.ครบถ้วน <br> 3.ตรงเวลา)
                                    </th>
                                    <th colspan="5" style="width: 15%;">(3) <br> ระดับค่าเป้าหมาย</th>
                                    <th rowspan="2" style="width: 7%;">(4) <br> ค่าคะแนน ที่ได้</th>
                                    <th rowspan="2" style="width: 10%;">(5) <br> น้ำหนัก (ความสำคัญ/ <br> ความยากง่ายของงาน)</th>
                                    <th rowspan="2" style="width: 10%;">(6) <br> ค่าคะแนน <br>ถ่วงน้ำหนัก <br> (4) X (5) 100</th>
                                </tr>
                                <tr>
                                    <th>1</th>
                                    <th>2</th>
                                    <th>3</th>
                                    <th>4</th>
                                    <th>5</th>
                                </tr>
                            </thead>
                            <tbody>
                                <template v-for="(item, index) in products_person" :key="index">
                                    <tr>
                                        <td style="text-align: left;" colspan="8">
                                            <b style="color: blue;">{{ item.id }}. {{ item.nameH }}</b>
                                        </td>
                                        <td class="text-center" style="color: blue;"><b>{{ item.h_weight }}%</b></td>
                                        <td></td>
                                        <td></td>
                                    </tr>
                                    <tr v-for="(subP01, idx) in item.subP01s" :key="idx" style="vertical-align: baseline;">
                                        <td class="text-center">
                                            <Checkbox :id="'checkOption-' + subP01.p_id" :name="'option-' + subP01.p_id" :value="subP01.p_id" v-model="checkboxValue" />
                                        </td>
                                        <td style="text-align: left;">{{ subP01.p_no }} {{ subP01.p_subject }}</td>
                                        <td style="text-align: left;">
                                            <b>ตัวชี้วัดที่ {{ idx+1 }} {{ subP01.p_subject }}</b>
                                            <p v-for="(subIitem, idI) in subP01.subITems" :key="idI" style="padding-left: 8px;margin-bottom: 5px;">
                                                <div v-if="subIitem.ind_no!=0"><b>ระดับ {{ subIitem.ind_no }}</b> {{ subIitem.ind_Items }}</div>
                                                <div v-if="subIitem.ind_no==0"><b>{{ subIitem.ind_Items }}</b></div>
                                            </p>
                                        </td>
                                        <td class="text-center">
                                            <b v-if="subP01.p_target==1">&#10003;</b>
                                            <b v-if="subP01.p_target!=1"></b>
                                        </td>
                                        <td class="text-center">
                                            <b v-if="subP01.p_target==2">&#10003;</b>
                                            <b v-if="subP01.p_target!=2"></b>
                                        </td>
                                        <td class="text-center">
                                            <b v-if="subP01.p_target==3">&#10003;</b>
                                            <b v-if="subP01.p_target!=3"></b>
                                        </td>
                                        <td class="text-center">
                                            <b v-if="subP01.p_target==4">&#10003;</b>
                                            <b v-if="subP01.p_target!=4"></b>
                                        </td>
                                        <td class="text-center">
                                            <b v-if="subP01.p_target==5">&#10003;</b>
                                            <b v-if="subP01.p_target!=5"></b>
                                        </td>
                                        <td class="text-center">{{ subP01.p_score }}</td>
                                        <td class="text-center">{{ subP01.p_weight }}%</td>
                                        <td class="text-center">{{ (subP01.p_score * subP01.p_weight / 100).toFixed(2) }}</td>
                                    </tr>
                                </template>
                            </tbody>
                        </table>
                    </div>
                </div>
            </form>
            <template #footer>
                <Button label="บันทึก" icon="pi pi-check" class="mb-2 mr-2" @click="savetablechkem" />
                <Button label="ยกเลิก" icon="pi pi-times" class="mb-2 mr-2" severity="danger" @click="cancelDialogs" />
            </template>
        </Dialog>
        <!-- เพิ่มข้อมูลแบบประเมิน -->
        <Dialog header="จัดการแบบ ป01" maximizable v-model:visible="DialogAdd" :breakpoints="{ '960px': '75vw' }" :style="{ width: '100vw' }" :modal="true" position="top">
            <form>
                <div class="p-fluid formgrid"> 
                    <form>
                        <InputText v-model="text_edt" type="hidden" />
                        <div class="p-fluid formgrid grid"> 
                            <div class="field col-12 md:col-4">
                                <label for="dropdownItemH">ประเภทภาระงาน</label>
                                <Dropdown id="dropdownItemH" v-model="dropdownItemH" :options="dropdownItemsH" optionLabel="nameH" placeholder="เลือกประเภทภาระงาน"></Dropdown>
                            </div>
                            <div class="field col-12 md:col-2">
                                <label for="text_no">ข้อที่</label>
                                <InputGroup>
                                    <InputGroupAddon>
                                        <i class="pi pi-calendar-plus"></i>
                                    </InputGroupAddon>
                                    <InputText v-model="text_no" type="number" placeholder="ข้อ" autocomplete="off" /> 
                                </InputGroup> 
                            </div>
                            <div class="field col-12 md:col-6">
                                <label for="text_name">เรื่อง</label>
                                <InputGroup>
                                    <InputGroupAddon>
                                        <i class="pi pi-book"></i>
                                    </InputGroupAddon>
                                    <Textarea v-model="text_name" rows="1" placeholder="เรื่อง" autocomplete="off" /> 
                                </InputGroup>  
                            </div> 
                            <div class="field col-12 md:col-3">
                                <label for="text_target">ระดับค่าเป้าหมาย</label>
                                <Dropdown id="dropdownItemTarget" v-model="dropdownItemTarget" :options="dropdownItemsTarget" optionLabel="name" placeholder="เลือกระดับค่าเป้าหมาย"></Dropdown>
                            </div> 
                            <div class="field col-12 md:col-4">
                                <label for="text_weight">น้ำหนัก(ความสำคัญ / ความยากง่ายของงาน)</label>
                                <InputGroup>
                                    <InputGroupAddon>
                                        <i class="pi pi-tags"></i>
                                    </InputGroupAddon>
                                    <InputText v-model="text_weight" type="number" placeholder="น้ำหนัก(ความสำคัญ / ความยากง่ายของงาน)" autocomplete="off" />  
                                </InputGroup>  
                            </div>  
                        </div> 
                        <hr>
                        <div class="p-fluid formgrid grid">
                            <div class="field col-12 md:col-12"> 
                                <label for="text_search_no">ตัวชี้วัด / เกณฑ์การประเมิน</label>
                                <InputGroup>  
                                    <InputText v-model="text_search_no" type="number" placeholder="ลำดับ" autocomplete="off" class="col-12 md:col-2" /> 
                                    <InputText v-model="text_search" type="text" placeholder="ชื่อตัวชี้วัด / เกณฑ์การประเมิน" autocomplete="off"/> 
                                    <Button icon="pi pi-save" label="บันทึก" severity="warning" @click="AddDatalist" />
                                </InputGroup>  
                            </div>   
                        </div> 
                        <DataTable :value="products_list" :rows="10" :paginator="true" responsiveLayout="scroll" dataKey="id">    
                            <Column field="ind_no" header="ลำดับ" style="width: 10%">
                                <template #body="Item">
                                    ระดับที่ {{ Item.data.ind_no }}
                                </template>
                            </Column> 
                            <Column field="ind_Items" header="ชื่อตัวชี้วัด / เกณฑ์การประเมิน" style="text-align: left;width: 80%">
                                <template #body="Item"> 
                                {{ Item.data.ind_Items }}
                                </template>
                            </Column>  
                            <Column field="options" header="ตัวเลือก" style="text-align: center; width: 10%">
                                <template #body="Item">   
                                    <Button severity="danger" icon="pi pi-trash" class="p-button-text" outlined rounded @click="DeleteRegislick(Item.data.ind_no)"></Button>
                                </template>
                            </Column> 
                        </DataTable>
                    </form>
                </div>
            </form>
            <template #footer>
                <Button label="บันทึก" icon="pi pi-check" class="mb-2 mr-2" @click="saveDatax" />
                <Button label="ยกเลิก" icon="pi pi-times" class="mb-2 mr-2" severity="danger" @click="cancelDialog" />
            </template>
        </Dialog>
    </div>  
</template>


<script> 
import { ref } from 'vue';
import axios from 'axios';  
import Swal from 'sweetalert2'
import { saveAs } from 'file-saver';

export default {
    data() {
        return { 
            staffid_Main: 5009942,
            year_Main: 2568,
            facid_Main: 201092704000,
            groupid_Main: '01',
            dropdownProportion: {name: 'สัดส่วน 70:30'},
            dropdownProportions: [
                { name: 'สัดส่วน 50:50', value: 50/50 },
                { name: 'สัดส่วน 70:30', value: 70/30 },
                
            ],
            dropdownItemYear: { name: 'ปีงบประมาณ 2568'},
            dropdownItemsYear: [
                { name: 'ปีงบประมาณ 2569', value: 2569 },
                { name: 'ปีงบประมาณ 2568', value: 2568 },
                { name: 'ปีงบประมาณ 2567', value: 2567 },
                { name: 'ปีงบประมาณ 2566', value: 2566 }
            ],
            products_personX: [],
            //products_person: [],
            checkboxValue: [],
            selectedItems: [],
            DialogAdd: false,
            DialogAddP01: false,
            //เพิ่มมา
            products_person: [],    
                itemsBtu: (item) => [
                    {
                        label: 'แก้ไขข้อมูล',
                        icon: 'pi pi-refresh',
                        command: () => {
                            this.editDatax(item);
                        }
                    },
                    {
                        label: 'ลบข้อมูล',
                        icon: 'pi pi-times',
                        command: () => {
                            this.delDatax(item);
                            
                        }
                    }
                ],
             // Dialog
            text_edt: null,
            dropdownItemH: null ,
            dropdownItemsH: [],
            text_no: null,
            text_name: null, 
            dropdownItemTarget: null ,
            dropdownItemsTarget: [
                { name: 'ระดับเป้าหมาย 1', value: 1 },
                { name: 'ระดับเป้าหมาย 2', value: 2 },
                { name: 'ระดับเป้าหมาย 3', value: 3 },
                { name: 'ระดับเป้าหมาย 4', value: 4 },
                { name: 'ระดับเป้าหมาย 5', value: 5 },
            ],
            text_weight: null,
            text_search_no: null,
            text_search: null,
            products_list: [],
        }
    }, 
    mounted(){ 
        this.showDataPerson(); 
    },
    methods: {  
        // ดึงข้อมูลเข้าตาราง
        showDataPerson(){
            axios.post('http://localhost:8000/api/showDataPersonX',{
            staff_id: this.staffid_Main,
            fac_id: this.facid_Main,
            year_id: this.year_Main, 
            group_id: this.groupid_Main,
            }).then(res => {     
                // console.log(res.data);  
                this.products_personX=res.data;
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }, 
        // ดึงข้อมูลมาแก้ไข
        async editDatax(data){    
          await axios.post('http://localhost:8000/api/edtDataPersonx',{
              p01_id: data.p01_id
          }).then(res => { 
                 //console.log(data);   
                    this.DialogAdd = true; 
                    this.text_edt = res.data[0].p01_id;   
                    this.text_no = res.data[0].p01_no; 
                    this.text_name = res.data[0].p01_subject;  
                    const target_f = this.dropdownItemsTarget.filter(f=>f.value==res.data[0].p01_target)
                    this.dropdownItemTarget = target_f.length > 0 ? target_f[0] : null;    
                    this.text_weight = res.data[0].p01_weight;  
                    this.products_list = res.data[0].sub_ITem; 
                    this.selectDataHEdt(res.data[0].p01_year,res.data[0].p01_facid,res.data[0].p01_h); 
          })
          .catch(error => {
              console.error('Error:', error);
          });
        },
        // ดึงข้อมูลภาระงาน
        selectDataHEdt(year,fac,he){  
            axios.post('http://localhost:8000/api/selectDataPersonH',{
                year: year,
                fac: fac,
            }).then(res => {     
                // console.log(res.data); 
                this.dropdownItemsH=res.data;  
                const h_f = res.data.filter(f=>f.id==he); 
                this.dropdownItemH = h_f.length > 0 ? h_f[0] : null;    

            })
            .catch(error => {
                console.error('Error:', error);
            });
        },
        // ลบข้อมูล
        async delDatax(data) {        
            Swal.fire({
                title: "คุณต้องการลบแบบ ป01 ใช่หรือไม่ ?",
                text: "เมื่อคลิกปุ่ม Yes, delete it! ข้อมูลจะถูกลบทันที!",
                icon: "warning",
                showCancelButton: true,
                confirmButtonColor: "#3085d6",
                cancelButtonColor: "#d33",
                confirmButtonText: "Yes, delete it!"
            }).then((result) => {
                if (result.isConfirmed) {
                    axios.post('http://localhost:8000/api/delDataPersonx', {
                        p01_id: data.p01_id
                    }).then(res => { 
                        this.showDataPerson(); 
                        Swal.fire({
                            title: "ลบข้อมูลเสร็จสิ้น!",
                            text: "ข้อมูลของคุณถูกลบแล้ว",
                            icon: "success"
                        });
                    }).catch(error => {
                        console.error('Error:', error);
                    }); 
                }
            }); 
        },

//*================== Start เลือกข้อมูลแบบประเมิน ป.01 ==================*//
        // เปิดหน้าต่างสำหรับบันทึก *ดึงข้อมูล
        OpenDialogP01(){ 
            axios.post('http://localhost:8000/api/showDataPerson', { 
                fac_id: this.facid_Main,
                year_id: this.year_Main
            }).then(res => {     
                this.DialogAddP01 = true; 
                this.products_person = res.data;
            })
            .catch(error => {
                console.error('Error:', error);
            }); 
        }, 
        // บันทึกข้อมูล ป01 *ดึงข้อมูล
        async savetablechkem() {    
            let x = []
            this.products_person.forEach((v) => {
                const selectedItems = v.subP01s.filter(item => this.checkboxValue.includes(item.p_id)); 
                x = x.concat(selectedItems);
            }) 
            // console.log(x.length);  
            if(x.length == 0){
                Swal.fire("ไม่มีข้อมูล","กรุณาเลือกข้อคำถามจาก ตัวจัดการ !","error");
            }else{
            await axios.post('http://localhost:8000/api/savePushDataP01',{
                data: x
            }).then(res => { 
                // console.log(res.data);  
                this.showDataPerson(); 
                this.DialogAddP01 = false; 
                Swal.fire({
                    title: "เรียบร้อย!",
                    text: "บันทึกข้อมูล แบบจัดการ ป01 เรียบร้อย!",
                    icon: "success"
                }); 
            })
            .catch(error => {
                console.error('Error:', error);
            });  
            }
        },
        // ปิดหน้าต่างสำหรับบันทึก *ดึงข้อมูล
        cancelDialogs(){
            this.DialogAddP01 = false; 
        },
//*================== End เลือกข้อมูลแบบประเมิน ป.01 ==================*//


//*================== Start เพิ่มข้อมูลแบบประเมิน ==================*//
        // เปิดหน้าต่างสำหรับบันทึก 
        OpenDialogAdd(){
            this.DialogAdd = true; 
            this.text_edt = null;
            this.dropdownItemH = null;
            this.text_no = null;
            this.text_name = null; 
            this.dropdownItemTarget = null;
            this.text_weight = null;
            this.text_search_no = null;
            this.text_search = null;
            this.products_list = []; 
            this.selectDataH(this.dropdownItemYear,this.facid_Main);
        },
        // ดึงข้อมูลภาระงาน
        selectDataH(year,fac){   
            axios.post('http://localhost:8000/api/selectDataPersonH',{
                year: year.value,
                fac: fac,
            }).then(res => {     
                // console.log(res.data); 
                this.dropdownItemsH=res.data;  
            })
            .catch(error => {
                console.error('Error:', error);
            });
        },
        // บันทึกตัวชี้วัด / เกณฑ์การประเมิน
        AddDatalist(){    
            if(this.text_search_no == null || this.text_search == null){ 
                Swal.fire("ไม่มีข้อมูล","กรุณาตรวจสอบข้อมูล ลำดับ - ชื่อตัวชี้วัด / เกณฑ์การประเมิน!","error");
            } else {    
                this.products_list.push({
                    ind_no: this.text_search_no,
                    ind_Items: this.text_search
                });   
                this.products_list.sort((a, b) => a.ind_no - b.ind_no);  
                this.text_search_no = null;
                this.text_search = null;
            } 
        },
        // ลบตัวชี้วัด / เกณฑ์การประเมิน
        DeleteRegislick(data){
            this.products_list = this.products_list.filter(product => product.ind_no !== data); 
        },
        // บันทึกแบบจัดการ ป.1 
        async saveDatax() {
            await axios.post('http://localhost:8000/api/saveDataP01User',{
                staff_id: this.staffid_Main,
                fac_id: this.facid_Main,
                year_id: this.dropdownItemYear.value,  
                text_edt: this.text_edt,
                dropdownItemH: this.dropdownItemH.id,
                text_no: this.text_no,
                text_name: this.text_name,
                text_target: this.dropdownItemTarget.value,
                text_weight: this.text_weight, 
                products_list: this.products_list
            }).then(res => {  
                // console.log(res.data); 
                this.DialogAdd = false;
                this.showDataPerson(); 
                Swal.fire({
                    title: "เรียบร้อย!",
                    text: "บันทึกข้อมูล แบบ ป01 เรียบร้อย!",
                    icon: "success"
                });
                
            })
            .catch(error => {
                console.error('Error:', error);
            });
 
        }, 
        // ปิดบันทึกแบบจัดการ ป.1 
        cancelDialog(){
            this.DialogAdd = false;
        },
//*================== End เพิ่มข้อมูลแบบประเมิน ==================*//

        // Export File
        exportToWord() {
            // สร้างเนื้อหาในไฟล์ Word เป็น HTML
            const content = `
                <html xmlns:o="urn:schemas-microsoft-com:office:office" xmlns:w="urn:schemas-microsoft-com:office:word" xmlns="http://www.w3.org/TR/REC-html40">
                <head><meta charset="utf-8"><title>Exported Word</title></head>
                <body>
                    <h1>รายงานการปฏิบัติงาน</h1>
                    <p>นี่คือเนื้อหาที่จะส่งออกไปเป็นไฟล์ Word</p>
                    <p>ข้อมูลเพิ่มเติมสามารถแทรกได้ที่นี่</p>
                </body>
                </html>
            `;

            // สร้าง Blob สำหรับไฟล์ Word
            const blob = new Blob(['\ufeff', content], {
                type: 'application/msword',
            });

            // ดาวน์โหลดไฟล์โดยใช้ file-saver
            saveAs(blob, 'report.doc');
            },


 
    } 
} 

 </script>

<style scoped> 
  label{
      font-size: medium;
      font-weight: 500;
  }
  .card-header {
      text-align: left; /* Aligns text to the left */
      margin: 0; /* Removes default margins */
      padding: 0; /* Removes default padding */
  }
  .table {
      width: 100%;
      border-collapse: collapse;
  }
  .table th, .table td {
      border: 1px solid #ddd;
      padding: 8px;
      text-align: center;
  }
  .table th {
      background-color: #f4f4f4;
      font-weight: bold;
  }
  .table td {
      vertical-align: top;
  }
</style>
