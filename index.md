## MelonDS-Emscripten

A SDL2/GLEW frontend for MelonDS I created in March 2021, compiled through Emscripten.


## Controls

A: X

B: Z

X: S

Y: A

L: Q

R: W

D-pad: Arrow Keys

<div id="needed" style="display:block;">
    Please upload the required files (they will be cached by your browser):
    <table id="requirelist">
        <tr id="bios7" style="display:none;"><td>BIOS7:</td><td><input type="file" id="bios7_file"></td></tr>
        <tr id="bios9" style="display:none;"><td>BIOS9:</td><td><input type="file" id="bios9_file"></td></tr>
        <tr id="firmware" style="display:none;"><td>Firmware:</td><td><input type="file" id="firmware_file"></td></tr>
    </table>
</div>

<table>
    <tr>
        <td>
            <button type="button" id="fullscreen">Fullscreen</button>
        </td>
        <td>
            <button type="button" id="save">Save</button>
        </td>
        <td>
            <button type="button" id="downloadsave">Export Save</button>
        </td>
        <td>
            <!-- Import Save:  -->
            <input type="file" id="inputsave" style="display:none;">
            <button type="button" id="opensave">Import Save</button>
            <!-- <button type="button" id="opensave">Import Save</button> -->
        </td>
    </tr>
</table>
<div class="emscripten"><progress id="progress" max="1" value="0" hidden=""></progress></div>
<div class="emscripten_border">
	<canvas class="emscripten" id="canvas" oncontextmenu="event.preventDefault()" tabindex="-1" width="650" height="580" style="cursor: default;"></canvas>
</div>
<script>
var statusElement=document.getElementById("status"),progressElement=document.getElementById("progress"),spinnerElement=document.getElementById("spinner"),
Module={
	preRun:[],
	postRun:[],
	print: function(text) {console.log("stdout: " + text);},
	printErr: function(text){ console.error("stderr: " + text); },
	canvas: document.getElementById("canvas"),
	totalDependencies:0,
};

//var canvas = document.getElementById("canvas");
//Module.setStatus("Downloading..."),
//window.onerror=function(e){Module.setStatus("Exception thrown, see JavaScript console"),spinnerElement.style.display="none",Module.setStatus=function(e){e&&Module.printErr("[post-exception status] "+e)}}

</script>
<script async="" src="melonDS.js"></script>

<script>
    function CacheSync(file, savedir) {
        var fr = new FileReader(); 
        fr.onload = function () {
            var data = new Uint8Array(fr.result);
            FS.writeFile(savedir, data);
            console.log("saved to " + savedir);
            //sync
            FS.syncfs(function (err) {
                assert(!err);
            });
            //recheck
            Module.ccall('check_required_files');
        };
        fr.readAsArrayBuffer(file);
    }

    let fb = document.getElementById('bios7_file');
    if(fb) {
        fb.addEventListener("change", function () {
            const fileList = this.files;
            if(fileList.length > 0) {
                const file = fileList[0];
                CacheSync(file, "saved/bios7.bin");
            }
        }, false);
    }

    fb = document.getElementById('bios9_file');
    if(fb) {
        fb.addEventListener("change", function () {
            const fileList = this.files;
            if(fileList.length > 0) {
                const file = fileList[0];
                CacheSync(file, "saved/bios9.bin");
            }
        }, false);
    }

    fb = document.getElementById('firmware_file');
    if(fb) {
        fb.addEventListener("change", function () {
            const fileList = this.files;
            if(fileList.length > 0) {
                const file = fileList[0];
                CacheSync(file, "saved/firmware.bin");
            }
        }, false);
    }


</script>