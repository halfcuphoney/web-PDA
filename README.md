# web-PDA
#this is a web personal-digital-account page where you can record your daiy life.
#it include diary and travel and picture three model of recording




<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html" charset="UTF-8">
        <title>合并</title>
        <link rel="stylesheet" href="l1-1.css">
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script><!--引入链接-->
        
        <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"></script>
        <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js"></script>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootswatch/4.1.2/minty/bootstrap.min.css">
        <link href="https://cdn.bootcss.com/animate.css/3.5.2/animate.min.css" rel="stylesheet">
        <style>
            #content{
                position: absolute;
                top: 0;
                left: 300px;
                bottom: 0;
                right: 0;
                overflow-y: auto;
                overflow-x: hidden;
            }
            .imgbox{
                width: 200px;
                height: 200px;
                border-radius: 50%;
                background-size: cover;
                background-position: top center;
                background-image: url('https://images.pexels.com/photos/794064/pexels-photo-794064.jpeg?auto=compress&cs=tinysrgb&h=350');
            }           
            #info-box{
                position: absolute;
                top: 0px;
                left: 0;
                bottom: 0;
                width: 300px;
                overflow-x: hidden;
                overflow-y: auto;
                z-index: 100;
            }
            .slide-enter-active,.slide-leave-active{
                transition: opacity 0.5s;
            }
            .slide-enter, .slide-leave-to{
                opacity:0;
            }
            /*解决照片定位错乱*/
            #pics-post-info .row div{
                display: flex;
                flex-direction: row;
                flex-wrap: wrap;
            }
    
            .row .card{
                width: 100%;
                height: 300px;
            }
            /*照片-动画*/
            .fade-enter-active,.fade-leave-ative{
                transition: all 3s ease-out;
            }
    
            .fade-enter{
                transform: translateX(500px);
                opacity: 0;
            }
            
            .fade-leave-active{
                transform: translateY(500px);
                opacity: 0;
            }
        </style>            
    </head>
    <body>
        <!--个人信息-->
        <div id="info-box" class="card" style="background-image:url('https://images.pexels.com/photos/997664/pexels-photo-997664.jpeg?auto=compress&cs=tinysrgb&h=350')">
            <center>
                <div class="card-body">
                    <h2 name="name" style="color:black">{{name}}</h2>
                    <div class="imgbox"></div>
                    <br/>
                    <div name="info" style="color:black">
                        <p>{{info[0]}}</p>
                        <p>{{info[1]}}</p>
                        <p>{{info[2]}}</p>
                    </div>

                    <a v-bind:href="content.adr">
                        <img v-bind:src="content.pic">
                    </a>
                </div>
            </center>
        </div>

        <div id="content">
            <!--导航栏-->
            <ul id="navigation" class="nav nav-tabs">
                <li class="nav-item" v-for="item in navigation">
                    <a data-toggle="tab" v-bind:href="item.href" class="nav-link">{{item.content}}</a>
                </li>
            </ul>

            <div class="tab-content">
                <!--日记-->
                <div id="dairy-post-info" class="tab-pane active show">
                    <div v-for="item in dairy" class="jumbotron">
                        <h2 name="post-title">
                            <span name="date" class="display-3 text-primary">{{item.date}}</span>
                            <span name="title">{{item.title}}</span>
                        </h2>
                        <p class="lead">{{item.ins}}</p>
                        <hr class="my-4">
                        <p>{{item.content}}</p>
                    </div> 
                    
                    <button class="btn btn-primary" v-on:click="showInput">添加日记</button>
                    <div v-bind:class="inputClass" role="document">
                        <div class="modal-content">
                            <div class="modal-body">
                                <div class="form-group row" v-for="(value,key) in item">
                                    <label class="col-sm-2 col-form-label">{{key}}</label>
                                    <div class="col-sm-10">
                                        <input type="text" class="form-control is-valid" v-model:value="item[key]">
                                    </div>
                                </div>
                            </div>
                            <div class="modal-footer">
                                <button class="btn btn-primary" v-on:click="submit('dairy')">提交</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!--游记-->
                <div id="travel-post-info" class="container-fluid tab-pane">
                    <div v-for="item in travel" class="jumbotron">
                        <div class="row">
                            <div class="col-md-5">
                                <img height="300" width="400" v-bind:src="item.img">
                            </div>
                            <div class="col-md-7">
                                <h2 name="post-title">
                                    <span name="date" class="display-3 text-primary">{{item.date}}</span>
                                    <br>
                                    <span name="title">{{item.title}}</span>
                                </h2>
                                <p class="lead">{{item.ins}}</p>
                                <p>{{item.content}}</p>
                                <hr class="my-4">
                            </div>
                        </div>
                    </div>

                    <button class="btn btn-primary" v-on:click="showInput">添加游记</button>
                    <div v-bind:class="inputClass" role="document">
                        <div class="modal-content">
                            <div class="modal-body">
                                <div class="form-group row" v-for="(value,key) in item">
                                    <label class="col-sm-2 col-form-label">{{key}}</label>
                                    <div class="col-sm-10">
                                        <input type="text" class="form-control is-valid" v-model:value="item[key]">
                                    </div>
                                </div>
                            </div>
                            <div class="modal-footer">
                                <button class="btn btn-primary" v-on:click="submit('travel')">提交</button>
                            </div>
                        </div>
                    </div>
                </div>
                    
                <!--照片集-->
                <div id="pics-post-info" class="container tab-pane">
                    <div class="row">
                        <div class="col-md-4" v-for="item in pics">
                            <div class="card">
                                <img v-bind:src="item.img" alt="Card image" class="card-img">
                                <div class="card-img-overlay" style="color:azure">
                                    <h4 class="card-title">{{item.title}}</h4>
                                    <p class="card-text">{{item.content}}</p>
                                    <p class="card-text" style="color:azure">
                                        <small class="text-muted">{{item.date}}</small>
                                    </p>
                                </div>
                            </div>
                            <br>
                        </div>
                    </div>

                    <button class="btn btn-primary" v-on:click="showInput">添加照片</button>
                    <div v-bind:class="inputClass" role="document">
                        <div class="modal-content">
                            <div class="modal-body">
                                <div class="form-group row" v-for="(value,key) in item">
                                    <label class="col-sm-2 col-form-label">{{key}}</label>
                                    <div class="col-sm-10">
                                        <input type="text" class="form-control is-valid" v-model:value="item[key]">
                                    </div>
                                </div>
                            </div>
                            <div class="modal-footer">
                                <button class="btn btn-primary" v-on:click="submit('pics')">提交</button>
                            </div>
                        </div>
                    </div>
                </div>

            </div>           
        </div>
        <script>
            var infoBoxApp = new Vue({
                el:"#info-box",
                data:{
                    name:"Alex",
                    info:["前端学习中","认真生活","每天记录好心情"],
                    content:{
                        adr:"https://www.weibo.com/",
                        pic:"https://images.pexels.com/photos/1439965/pexels-photo-1439965.jpeg?cs=srgb&dl=chair-contemporary-cushions-1439965.jpg&fm=jpg",
                    }
                }
            });

            var contentApp = new Vue({
                el: "#content",
                methods:{
                    showInput:function(event){
                        if (this.inputClass === "fade modal-dialog"){
                            this.inputClass="fade madal-dialog active show"
                        }
                        else{
                            this.inputClass="fade modal-dialog"
                        }
                    },
                    submit:function(message){
                        if (message==="dairy"){
                            this.dairy.push(Object.assign({},this.item))
                        }
                        else if(message ==="travel"){
                            this.travel.push(Object.assign({},this.item))
                        }
                        else{
                            this.pics.push(Object.assign({},this.item))
                        }
                        this.item.date="填入日期"
                        this.item.title="标题"
                        this.item.ins="题记"
                        this.item.img="填入图片地址，若是添加“日记”，则可忽略不填"
                        this.item.content="内容"
                        this.inputClass="fade modal-dialog"
                        
                    }
                },
                data:{
                    inputClass:"fade modal-dialog",
                    item:{
                        img:"填入图片地址，若是添加“日记”，则可忽略不填",
                        date:"填入日期",
                        title:"标题",
                        ins:"题记",
                        content:"内容",
                    },
                    navigation:[
                        {
                            content:"日记",
                            href:"#dairy-post-info"
                        },
                        {
                            content:"游记",
                            href:"#travel-post-info"
                        },
                        {
                            content:"照片集",
                            href:"#pics-post-info"
                        }
                    ],
                    dairy:[
                        {
                            date: "2018/7/28",
                            title:"the first day to learn vue",
                            ins: "Inscription",
                            content: "easy-to-gowith-Vue.js",
                        },
                        
                    ],
                    travel:[
                        {
                            img:"https://cdn.pixabay.com/photo/2017/11/20/20/12/helicopter-2966569__340.jpg",
                            date:"2018/07/30",
                            title:"travel around world",
                            ins:"inscription",
                            content:"Great time",
                        },
                    ],
                    pics: [
                        {
                            img: "https://cdn.pixabay.com/photo/2018/10/04/14/22/donut-3723751__340.jpg",
                            title: "Pics story",
                            content: "This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.",
                            date: "update 2018/07/30"
                        },
                        {
                            img: "https://cdn.pixabay.com/photo/2018/05/11/23/45/highway-3392100__340.jpg",
                            title: "Pics story",
                            content: "This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.",
                            date: "update 2018/07/30"
                        },
                        {
                            img: "https://cdn.pixabay.com/photo/2018/05/30/16/23/fruit-3441830__340.jpg",
                            title: "Pics story",
                            content: "This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.",
                            date: "update 2018/07/30"
                        },
                        {
                            img: "https://cdn.pixabay.com/photo/2018/06/04/23/42/raspberry-3454504_1280.jpg",
                            title: "Pics story",
                            content: "This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.",
                            date: "update 2018/07/30"
                        },
                        {
                            img: "https://cdn.pixabay.com/photo/2018/07/14/22/53/currants-3538617_1280.jpg",
                            title: "Pics story",
                            content: "This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.",
                            date: "update 2018/07/30"
                        },

                    ],
                },
                
            })
        </script>
    </body>
</html>
