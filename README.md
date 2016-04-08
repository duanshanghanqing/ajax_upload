# 一个ajax文件上传demo

## 使用方法

### html
    <input type="file" name="myfile" value="" id="ajax_myfile" multiple/><br/><br/>
    <input type="button" value="上传按钮" id="ajax_button"><br/><br/>
    <input type="button" value="停止上传" id="ajax_stop"><br/><br/>


### script
    <script src="ajax_upload.js"></script>
    <script>
        var ajax_button = document.getElementById("ajax_button");
        ajax_button.onclick=function(){
            var files = document.getElementById("ajax_myfile").files
            var  res = multiFileUpload({
                url:"/upload3",
                files:files,//上传的文件
                uploaduStart:function(event){//开始上传
                    console.log('开始上传');
                },
                uploadedBeing:function(event){//上传进度事件，文件在上次的过程中，会多次触发该事件，返回一个event事件对象
                    if (event.lengthComputable) {//返回一个  长度可计算的属性，文件特别小时，监听不到，返回false
                        //四舍五入
                        var percent = Math.round(event.loaded * 100 / event.total);//event.loaded:表示当前已经传输完成的字节数。
                        //event.total:当前要传输的一个总的大小.通过传输完成的除以总的，就得到一个传输比率
                        console.log('进度', percent);
                    }
                },
                uploadSuccess:function(event){//上传成功
                    console.log('上传成功');
                    //console.log(xhr.responseText);//得到服务器返回的数据
                },
                uploadError:function(event){//上传出错
                    console.log('发生错误');
                },
                uploadCancelled:function(event){//取消上传
                    console.log('操作被取消');
                },
                uploadEnd:function(event){//上传结束
                    console.log('传输结束，不管成功失败都会被触发');
                },
                serviceCallback:function(data){//服务器回掉返回的数据
                    console.log(data);
                }
            });
    
            var ajax_stop = document.getElementById("ajax_stop");
            ajax_stop.onclick=function(){
                res.abort();
            }
        }
    
    </script>