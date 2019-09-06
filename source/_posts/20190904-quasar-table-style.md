---
title: q-table에서 특정 행에 스타일 적용하기
categories:
  - front-end
  - vue.js
  - quasar
tags:
  - quasar
  - q-table
  - q-tr
  - q-td
date: 2019-09-04 11:21:03
thumbnail:
---
### q-table 사용
  - quasar에서 제공하는 table 컴포넌트( https://quasar.dev/vue-components/table )
  - 실시간 리스트용으로 간단한 테이블이 필요했다. 기능은 실시간 조회만 가능하면 되기때문에 단순html로 구현을 할까 했지만 스타일적용이나 정렬기능도 필요할것 같아서 q-table로 진행하였다.
  <!--more-->

### 기능에 따른 스타일 적용    
  - 조회된 최신 데이타가 테이블 최상단에 추가
  - 사용자로 하여금 최신 데이타로 인지 되도록 스타일 적용
  
### 시도
  - q-table에서 selectedRowIndex(?)와 같은 기능이 있을까 싶어 열심히 찾아 보았지만 역시나 없다.
  - 테이블에서 마우스 오버시 발생하는 이벤트를 던져볼까도 생각해봤지만 이 역시 무산.
  - 여기저기 구글링 한결과 `<q-tr>` `<q-td>` 를 이용해서 해결하였다.

### 해결
  - `<q-table>` 으로 테이블 구성시 `<q-tr>`,`<q-td>` 내부 구현으로 해결.
  - 아래 코드 처럼  props.row.__index를 이용해 원하는 위치에 스타일을 적용시키면 된다                  
  ```html
      <template>
        <div class="q-pa-md"> 
          <q-table
            ref="dtable"        
            dense
            dark
            color="primary"      
            card-class="bg-blue-grey-10 text-grey"        
            hide-bottom
            :data="attackData"
            :columns="columns"
            :pagination.sync="pagination"        
            row-key="time"            
          >
            <template v-slot:body="props">
              <q-tr :props="props">                                    
                <q-td
                  v-for="col in props.cols"              
                  :key="col.name"
                  :props="props"              
                  :class="props.row.__index===0?'q-table-selectRow':''" 
                >
                  {{ col.value }}
                </q-td>
              </q-tr>
            </template>  
          </q-table>
        </div>
      </template>
    ```
    ```js
      <script>
        export default {
            name: 'RealTimeDataTable',
            props:{
              attackData:{
                    type: Array,
                    default: () => []
                }
            },    
            data(){
                return {                    
                    pagination:{          
                        sortBy:'time',
                        descending:false,
                        page:1,
                        rowsPerPage:5
                    },
                    columns: [
                        {
                        name: 'time',
                        required: true,
                        label: 'TIME',
                        align: 'left',
                        field: row => row.time,
                        format: val => `${val}`,
                        sortable: true
                        },
                        { name: 'attack', align: 'center', label: 'ATTACK', field: 'attack', sortable: true, sort: (a, b) => parseInt(a, 10) - parseInt(b, 10) },
                        { name: 'type', align: 'center', label: 'TYPE', field: 'type', sortable: true },
                        { name: 'source', label: 'SOURCE', field: 'source', sortable: true },
                        { name: 'target', label: 'TARGET', field: 'target', sortable: true }
                    ]
                }
            
            
            }
        }
      </script>    
    ```
    ```css  
        .q-table-selectRow {
          background-color: red;            
        }
    ```