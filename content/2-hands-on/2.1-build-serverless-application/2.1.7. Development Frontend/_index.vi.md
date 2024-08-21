---
title : "Hướng Dẫn Phát Triển Frontend Cho Ứng Dụng Serverless"
description : "Khám phá hướng dẫn chi tiết về phát triển frontend cho ứng dụng serverless. Tìm hiểu cách xây dựng và tối ưu hóa giao diện người dùng cho các kiến trúc serverless."
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 2.1.7 </b> "
image: "images/2/FE1.jpeg"
---


## Các phương pháp hay nhất để phát triển giao diện người dùng

![Phát triển giao diện người dùng](/images/2/FE1.jpeg?featherlight=false&width=80pc)

Sau khi hoàn thành front-end chúng ta có một website tương tự như hình trên, các bạn có thể tạo, chỉnh sửa, xóa ghi chú.

Trang web Url công khai: **https://daotq2000.github.io/note-app/**

Kho lưu trữ Github: https://github.com/daotq2000/note-app.git

#### Thực hành
Hãy kiểm tra mã nguồn cấu trúc

     ├── chỉ mục.css
     ├── chỉ mục.html
     ├──index.js
     └── README.md
Chúng ta cần tạo 3 tập tin:
+ index.html: xác định cấu trúc cho website, các phần tử HTML
+ index.js: chứa script có JavaScript để Fetch HTTP request và tương tác với API.
+ index.css: dùng để xác định màu sắc, định dạng cho HTML Elmement.

**File index.html**

    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>notes taking app</title>


    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" rel="stylesheet" >
    <link rel="stylesheet" href="index.css" />
    </head>


    <body>
    <h1 style="text-align: center;color: white;">AWS handson labs for serverless application make by TRAN QUANG DAO</h1>
    <div class="popup-box">
        <div class="popup">
            <div class="content">
                <header>
                    <p>Add a new note</p>
                    <i class="fa fa-times" aria-hidden="true"></i>
                </header>
                <form action="#">
                    <div class="row title">
                    <label for="">Title</label>
                    <input type="text"  id="title">
                    </div>
                    <div class="row description">
                    <label for="">Description</label>
                    <textarea id="description"></textarea>
                    </div>
                    <button> Save Note</button>
                </form>
            </div>
        </div>
    </div>
    <div class="wrapper">
        <li class="add-box">
        <div class="icon"><i class="fa-solid fa-plus"></i></div>
        <p>Add new note</p>
        </li>
    <!-- <li class="note">
        <div class="details">
            <p>this is title</p>
            <span>Lorem ipsum dolor sit amet consectetur adipisicing elit. Distinctio, nobis!lorem10
                Lorem ipsum dolor sit amet Lorem ipsum dolor .
            </span>
        </div>
        <div class="bottom-content">
            <span>9 Sep 2022</span>
            <div class="settings show">
                <i class="fa-solid fa-ellipsis hide"></i>
                <ul class="menu">
                    <li><i class="fa-light fa-pen"></i>Edit</li>
                    <li><i class="fa-duotone fa-trash"></i>Delete</li>
                </ul>
            </div>
        </div>
    </li> -->
    
    </div>

**File  index.css**

    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600;900&display=swap');


    *{
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    }


    body{
    font-family: 'Poppins', sans-serif;
    background-color: #88abff;
    }


    .wrapper{
    margin: 50px;
    display: grid;
    grid-template-columns: repeat(auto-fill , 265px);
    gap: 15px;
    }


    .wrapper li{
    background-color: #fff;
    list-style: none;
    height: 250px;
    border-radius: 5px;
    padding: 15px 20px 20px;
    }
    .wrapper .note{
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    }


    .add-box , .icon , .bottom-content {
    display: flex;
    justify-content: center;
    align-items: center;   
    flex-direction: column;
    }


    .add-box .icon{
    height: 78px;
    width: 78px;
    border-radius: 50%;
    border: 2px dashed powderblue;
    font-size: 40px;
    color: #88abff;
    cursor: pointer;
    }


    .add-box p{
    font-weight: 500;
    margin-top: 20px;
    color: #88abff;
    }


    .note p{
    font-size: 22px;
    font-weight: 500;
    }


    .note span{
    display: block;
    font-size: 16px;
    color: #575757;
    margin-top: 5px;
    }


    .note .bottom-content{
    flex-direction: row;
    justify-content: space-between;
    padding-top: 10px;
    border-top: 1px solid #ccc;
    }


    .bottom-content span{
    color: #6d6d6d;
    font-size: 14px;
    }


    .bottom-content .settings i{
    cursor: pointer;
    font-size: 15px;
    color: #6d6d6d;
    }




    .settings{
    position: relative;
    }


    .settings .menu {
    box-shadow: 0 0 6px rgba(0,0,0,0.15);
    position:absolute;
    bottom:0;
    padding: 5px 0;
    border-radius: 4px;
    background-color: #fff;
    right: -4px;
    
    transform:scale(0);
    transform-origin: bottom right;
    transition: transform 0.2s ease;
    }
    .settings.show .menu{
    transform: scale(1);
    }


    .settings .menu li{
    height: 25px;
    border-radius: 0;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    cursor: pointer;
    font-size: 16px;
    padding: 17px 15px;
    }




    .menu li i{
    padding-right: 8px;
    }


    .menu li:hover{
    background-color: #f5f5f5;
    }


    .popup-box{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.4);
    }


    .popup-box .popup{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50% ,-50%);
    z-index: 2;
    max-width: 400px;
    width: 100%;
    }


    .popup .content{
    background-color: #fff;
    border-radius: 5px;
    }


    .popup .content header{
    border-bottom: 1px solid #ccc;
    display: flex;
    justify-content: space-between;
    padding: 15px 25px;
    }


    .popup .content header p{
    font-weight: 500;
    font-size: 20px;
    }


    .popup .content header i{
    font-size: 23px;
    cursor: pointer;
    color: #8b8989;
    }


    .content form{
    margin: 15px 25px 35px;
    }


    .content form :where(input , textarea){
    width: 100%;
    height: 50px;
    outline:none;
    font-size: 17px;
    border-radius: 4px;
    border: 1px solid #999;
    padding: 0 15px;
    }


    form .row label{
    margin-bottom: 6px;
    font-size: 18px;
    display: block;
    }
    .content form textarea{
    height: 150px;
    resize: none;
    padding: 8px 15px;;
    }


    .content form button{
    background-color: #6a93f8;
    height: 50px;
    width: 100%;
    margin: 15px 0;
    border: none;
    font-size: 17px;
    cursor: pointer;
    border-radius: 4px;
    color: #fff;
    }


    .popup-box , .popup-box.popup{
    opacity: 0;
    pointer-events: none;
    transition: all 0.25s ease;
    }


    .popup-box.show {
    opacity: 1;
    pointer-events:auto;
    }


    .hide-icon{
    display: none;
    }
**File index.js**

**Lưu ý:** Hãy thay thế  BACK_END_URI của bạn bằng các biến trong tệp index.js và thay thế Url gọi của bạn

+ const BACK_END_URI = “https://q3q8l57ui9.execute-api.us-east-1.amazonaws.com/dev“;

+ const BACK_END_URI = “url của bạn”;


        const BACK_END_URI = "https://q3q8l57ui9.execute-api.us-east-1.amazonaws.com/dev";
        const COMMON_NOTE_URI = `${BACK_END_URI}` + "/notes"

        const addBox = document.querySelector(".add-box")
        const popupBox = document.querySelector(".popup-box")

        const months = ['jan', 'feb', 'mar', 'apr', 'may', 'jun', 'jul', 'aug', 'sep', 'oct', 'nov', 'dec']

        const closeBox = popupBox.querySelector("header i")
        const titleTag = popupBox.querySelector("input")
        const descTag = popupBox.querySelector("textarea")
        const addBtn = popupBox.querySelector("button")
        let existingId = null;
        const notes = JSON.parse(localStorage.getItem('notes') || '[]')

        const menuel = document.querySelector('.iconel')

        const showNotes = () => {
            fetch(COMMON_NOTE_URI, {
                method: 'GET'
            })
                .then(response => response.json())
                .then(response => {

                    document.querySelectorAll('.note').forEach(note => note.remove())
                    response.forEach((note, index) => {
                        let litag = `<li class="note">
                                    <div class="details">
                                        <p> ${note.title} </p>
                                        <span>${note.description}
                                        </span>
                                    </div>
                                    <div class="bottom-content">
                                        <span>${note.date != null ? new Date(note.date).toLocaleDateString() + " " + new Date(note.date).toLocaleTimeString() : ""}</span>
                                        <div class="settings">
                                            <i onclick=showMenu(this) class="fa-solid fa-ellipsis iconel"></i>
                                            <ul class="menu">
                                                <li onclick="editNote(${index},'${note.id}','${note.title}','${note.description}')"><i class="fa-light fa-pen"></i>Edit</li>
                                                <li onclick="deleteNote(${index},'${note.id}')"><i class="fa-duotone fa-trash"></i>Delete</li>
                                            </ul>
                                        </div>
                                    </div>
                            </li>`

                        addBox.insertAdjacentHTML('afterend', litag)
                    })
                })
        }

        function showMenu(elem) {
            elem.parentElement.classList.add('show')
            document.onclick = (e) => {
                if (e.target.tagName != 'I' || e.target != elem) {
                    elem.parentElement.classList.remove('show')
                }
            }
            // console.log(elem)
        }

        function deleteNote(index, noteId) {

            fetch(COMMON_NOTE_URI, {
                method: 'DELETE',
                body: JSON.stringify({ id: noteId })
            })
                .then(response => response.json())
                .then(response => {
                    alert(response);
                    showNotes();
                })
        }

        function editNote(index, noteId, title, description) {
            popupBox.classList.add("show");
            existingId = noteId;
            document.getElementById("title").value = title;
            document.getElementById("description").value = description;
        }

        addBox.onclick = () => {
            popupBox.classList.add("show");
        }
        closeBox.onclick = () => {
            titleTag.value = ''
            descTag.value = ''
            popupBox.classList.remove("show");

        }
        addBtn.onclick = (e) => {
            console.log(e);
            e.preventDefault()
            //    menuel.classList.add('hide-icon')

            let title = titleTag.value;
            let desc = descTag.value;
            let noteInfo = {
                id: `${existingId ? existingId : generateUUID()}`,
                title: title,
                description: desc,
                date: `${new Date()}`
            }

            fetch(COMMON_NOTE_URI, {
                method: 'POST',
                body: JSON.stringify(noteInfo)
            })
                .then(response => response.json())
                .then(response => {
                    alert('Save successfully')
                    showNotes();
                })

            closeBox.click();
        }

        function generateUUID() { // Public Domain/MIT
            var d = new Date().getTime();//Timestamp
            var d2 = ((typeof performance !== 'undefined') && performance.now && (performance.now() * 1000)) || 0;//Time in microseconds since page-load or 0 if unsupported
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                var r = Math.random() * 16;//random number between 0 and 16
                if (d > 0) {//Use timestamp until depleted
                    r = (d + r) % 16 | 0;
                    d = Math.floor(d / 16);
                } else {//Use microseconds since page-load if supported
                    r = (d2 + r) % 16 | 0;
                    d2 = Math.floor(d2 / 16);
                }
                return (c === 'x' ? r : (r & 0x3 | 0x8)).toString(16);
            });
        }
        showNotes()
        console.log(existingId);