<script>
    import domtoimage from 'dom-to-image';
    import FileSaver from "file-saver";
    export default {
        data() {
            return {
                window_type: 'left',
                theme_type: 'dark',
                card_title:'businessCard.json',
                rotate_x:0,
                rotate_y:0,
                info_list: [
                    {title:'name',value:'zhoujump'},
                    {title:'job',value:'Front-end engineers'},
                    {title:'e-mail',value:'x@zhoujump.club'},
                ],
                saving:false,
            }
        },
        methods: {
            // 删除信息项的方法
            deleteInfo(index) {
                this.info_list.splice(index, 1);
            },
            // 添加信息项的方法
            addInfo() {
                this.info_list.push({ title: '', value: '' });
            },
            //
            mousemove(e) {
                let x = e.offsetX;
                let y = e.offsetY;
                let width = document.getElementById('view-cont').offsetWidth;
                let height = document.getElementById('view-cont').offsetHeight;
                this.rotate_x = (x / width) - 0.5;
                this.rotate_y = (y / height) - 0.5;
            },
            // 保存
            savepic() {
                this.saving=true;
                setTimeout(()=>{
                    domtoimage.toPng(document.getElementById('business-card'))
                    .then( blob => {
                        this.saving=false;
                        FileSaver.saveAs(blob, this.card_title+'.png');
                    });
			    },30)
            }
        }
    }
</script>

<template>
    <div class="editor">
        <div class="head-bar">
            <div class="app-title">
                码农名片生成器
            </div>
            <a target="_blank" class="app-button" href="https://github.com/ZhouJump/businesscard">
                GitHub  
            </a>
            <a target="_blank" class="app-button" href="https://zhoujump.club">
                更多项目  
            </a>
            <a target="_blank" class="app-button" href="https://blog.zhoujump.club">
                关于我  
            </a>
        </div>
        <div class="body-cont">
            <div @mousemove="mousemove" id="view-cont" class="view-cont">
                <div id="business-card" :style="{'--x':rotate_x,'--y':rotate_y}" :class="['business-card',theme_type,saving?'saving':'']">
                    <div :style="{flexDirection:window_type==='right'?'row-reverse':''}" class="card-header">
                        <div class="button-mac">
                            <div class="button-1"></div>
                            <div class="button-2"></div>
                            <div class="button-3"></div>
                        </div>
                        <div class="card-title">{{ card_title }}</div>
                    </div>
                    <div class="card-toolbox">
                        <img src="/src/assets/file-code.png" />
                        <img src="/src/assets/fork.png" />
                        <img src="/src/assets/application.png" />
                    </div>
                    <div class="card-body">
                        <div class="card-info out">
                            <div class="num">1</div>
                            <span class="tab"></span>{
                        </div>
                        <div v-for="(info,index) in info_list" class="card-info info">
                            <div class="num">{{ index+2 }}</div>
                            <span class="tab"></span>
                            <span class="key">"{{ info.title }}"</span>:
                            <span class="value">"{{ info.value }}"</span>,
                        </div>
                        <div class="card-info out">
                            <div class="num">{{ info_list.length+2 }}</div>
                            <span class="tab"></span>}
                        </div>
                    </div>
                </div>
            </div>
            <div class="edit-cont">
                <div class="input">
                    <div class="lable">
                        标题
                    </div>
                    <input type="text" placeholder="请输入标题" v-model="card_title">
                </div>
                <div class="hr"></div>
                <div class="input">
                    <div class="lable">
                        信息
                    </div>
                    <div class="list-count">
                        <div v-for="(item,index) in info_list" class="list-item">
                            <div class="input">
                                <div class="lable">
                                    键
                                </div>
                                <input type="text" v-model="item.title" placeholder="请输入键">
                            </div>
                            <div class="input">
                                <div class="lable">
                                    值
                                </div>
                                <input type="text" v-model="item.value" placeholder="请输入值">
                            </div>
                            <div class="del" @click="deleteInfo(index)">⨉</div>
                        </div>
                        <div class="add" @click="addInfo">+</div>
                    </div>
                </div>
                <div class="hr"></div>
                <div class="input">
                    <div class="lable">
                        按钮位置
                    </div>
                    <div class="radio">
                        <div @click="window_type='left'" class="radio-item" :class="window_type==='left'?'active':''">左侧</div>
                        <div @click="window_type='right'" class="radio-item" :class="window_type==='right'?'active':''">右侧</div>
                    </div>
                </div>
                <div class="hr"></div>
                <div class="input">
                    <div class="lable">
                        主题配色
                    </div>
                    <div class="radio">
                        <div @click="theme_type='dark'" class="radio-item" :class="theme_type==='dark'?'active':''">暗色</div>
                        <div @click="theme_type='light'" class="radio-item" :class="theme_type==='light'?'active':''">亮色</div>
                    </div>
                </div>
                <div class="hr"></div>
                <div class="input">
                    <div class="list-count">
                        <div class="add" @click="savepic">保存为图片</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .app-title{
        color: #D7D7D7;
        font-size: 18px;
        font-weight: 500;
        margin-right: auto;
    }
    .app-button{
        height: 30px;
        display: flex;
        padding: 0 16px;
        font-size: 14px;
        align-items: center;
        background-color: #242424;
        color: #D7D7D7;
        text-decoration: none;
        outline: 1px solid #454545;
        border-radius: 4px;
        
    }
    .edit-cont .hr{
        height: 1px;
        background-color: #2B2B2B;
        margin: 8px 0;
    }
    .input .lable{
        color: #D7D7D7;
        font-size: 14px;
        padding-bottom: 4px;
    }
    .input .radio{
        display: flex;
        grid-gap: 8px;
    }
    .radio .radio-item{
        text-align: center;
        flex: 1;
        color: #D7D7D7;
        font-size: 14px;
        cursor: pointer;
        padding: 2px 4px;
        border-radius: 4px;
        background-color: #242424;
        outline: 1px solid #454545;
    }
    .radio-item.active{
        background-color: #D7D7D7;
        color: #242424;
    }
    .list-count .add{
        color: #D7D7D7;
        font-size: 14px;
        cursor: pointer;
        width: 100%;
        outline: 1px solid #454545;
        padding: 2px 4px;
        text-align: center;
        background-color: #242424;
        border-radius: 4px;
        box-sizing: border-box;
    }
    .input .list-item{
        display: flex;
        grid-gap: 8px;
        align-items: center;
        margin-bottom: 8px;
    }
    .list-item .input{
        display: flex;
        flex: 1;
    }
    .list-item .input .lable{
        margin-right: 8px;
    }
    .list-item .del{
        color: #D7D7D7;
        font-size: 14px;
        cursor: pointer;
    }
    .input input{
        transition-duration: 200ms;
        box-sizing: border-box;
        width: 100%;
        background-color: #242424;
        border: none;
        outline: 1px solid #454545;
        padding: 4px 8px;
        border-radius: 4px;
        color: #D7D7D7;
        font-size: 16px;
    }
    .input input:focus{
        outline: 1px solid #D7D7D7;
    }
    .input input:hover{
        outline: 1px solid #D7D7D7;
    }
    .view-cont {
        user-select: none;
        flex: 1;
        background: radial-gradient(#292C34, #181A1F);
        border-radius: 8px;
        display: flex;
        align-items: center;
        justify-content: center;
        perspective: 300px;
    }
    .card-body{
        background-color: #1F1F1F;
        display: flex;
        flex: 1;
        flex-direction: column;
        counter-reset: info 0;
    }
    .light .card-body{
        background-color: #FFFFFF;
    }
    .card-info{
        font-size: 14px;
        color: #D7D7D7;
        display: flex;
        height: 24px;
    }
    .light .card-info{
        color: #3B3B3B;
    }
    .card-info.out{
        color: #DA70D6;
    }
    .light .card-info.out{
        color: #319331;
    }
    .card-info.info .tab{
        width: 24px;
    }
    .card-info.out .tab{
        width: 8px;
    }
    .card-info.info .key{
        color: #9CDCFE;
    }
    .light .card-info.info .key{
        color: #0451A5;
    }
    .card-info.info .value{
        color: #CE9178;
    }
    .light .card-info.info .value{
        color: #A31515;
    }
    .card-info .num {
        padding: 0 6px 0 12px;
        content: counter(info);
        counter-increment: info;
        height: 100%;
        display: block;
        color: #6E7681;
        background-color: #24262B;
    }
    .light .card-info .num {
        color: #A3A3A3;
        background-color: #ECECEC;
    }
    .card-info:last-child{
        flex: 1;
    }
    .card-toolbox {
        display: flex;
        padding: 6px 16px;
        border-bottom: 1px solid #2B2B2B;
        grid-gap: 8px;
    }
    .light .card-toolbox {
        border-bottom: 1px solid #E5E5E5;
    }
    .card-toolbox img {
        width: 18px;
        height: 18px;
        filter: invert(0.6);
    }
    .card-header {
        color: #D7D7D7;
        padding: 6px 16px;
        font-size: 14px;
        border-bottom: 1px solid #2B2B2B;
        display: flex;
        align-items: center;
        grid-gap: 26px;
    }
    .light .card-header {
        color: #3B3B3B;
        border-bottom: 1px solid #E5E5E5;
    }
    .card-header .card-title {
        flex: 1;
    }
    .button-mac{
        display: flex;
        grid-gap: 5px;
        align-items: center;
        justify-content: center;
    }
    .button-1,.button-2,.button-3 {
        width: 12px;
        height: 12px;
        border-radius: 50%;
        background-color: #FF6057;
    }
    .button-2 {
        background-color: #FFBD2E;
    }
    .button-3 {
        background-color: #27C93F;
    }
    .business-card {
        position: relative;
        pointer-events: none;
        display: flex;
        flex-direction: column;
        overflow: hidden;
        border-radius: 8px;
        width: 460px;
        aspect-ratio: 90/54;
        background-color: #181818;
        transform: rotate3d( calc(var(--y) * -1),var(--x), 0, 3deg);
        overflow: hidden;
    }
    .business-card.saving {
        transform: none;
    }
    .light.business-card {
        background-color: #F8F8F8;
    }
    .edit-cont {
        display: flex;
        flex-direction: column;
        grid-gap: 8px;
        box-sizing: border-box;
        padding: 16px;
        width: 30%;
        min-width: 300px;
        background-color: #181A1F;
        border-radius: 8px;
    }
    .editor {
        height: 100%;
        display: flex;
        flex-direction: column;
    }
    .head-bar {
        grid-gap: 10px;
        display: flex;
        align-items: flex-end;
        height: 40px;
        padding: 0 16px;
        box-sizing: border-box;
    }
   .body-cont {
        padding: 10px;
        flex: 1;
        display: flex;
        grid-gap:10px;
    }
</style>