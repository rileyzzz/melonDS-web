## MelonDS-Emscripten

A SDL2/GLEW frontend for MelonDS I created in March 2021, compiled through Emscripten.


## Controls
<table>
<tr><td>A:</td><td>X</td></tr>

<tr><td>B:</td><td>Z</td></tr>

<tr><td>X:</td><td>S</td></tr>

<tr><td>Y:</td><td>A</td></tr>

<tr><td>L:</td><td>Q</td></tr>

<tr><td>R:</td><td>W</td></tr>

<tr><td>D-pad:</td><td>Arrow Keys</td></tr>

<tr><td>Select:</td><td>Right Shift</td></tr>

<tr><td>Start:</td><td>Enter</td></tr>
</table>

<div id="needed" style="display:block;">
    Please upload the required files (they will be cached by your browser):
    <table id="requirelist">
        <tr id="bios7" style="display:none;"><td>BIOS7:</td><td><input type="file" id="bios7_file"></td></tr>
        <tr id="bios9" style="display:none;"><td>BIOS9:</td><td><input type="file" id="bios9_file"></td></tr>
        <tr id="firmware" style="display:none;"><td>Firmware:</td><td><input type="file" id="firmware_file"></td></tr>
    </table>
</div>

<div id="controls">
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
	<table>
		<tr>
			<td>
				Screen Size:
				<select id="size">
					<option value="1x">1x</option>
					<option value="2x" selected>2x</option>
					<option value="3x">3x</option>
					<option value="4x">4x</option>
				</select>
			</td>
			<td>
				Screen Rotation:
				<select id="rotation">
					<option value="0">0&#176;</option>
					<option value="90">90&#176;</option>
					<option value="180">180&#176;</option>
					<option value="270">270&#176;</option>
				</select>
			</td>
			<td>
				Screen Gap:
				<select id="gap">
					<option value="0">0px</option>
					<option value="1">1px</option>
					<option value="8">8px</option>
					<option value="64">64px</option>
					<option value="90">90px</option>
					<option value="128">128px</option>
				</select>
			</td>
			<td>
				Screen Layout:
				<select id="layout">
					<option value="natural">Natural</option>
					<option value="vertical">Vertical</option>
					<option value="horizontal">Horizontal</option>
				</select>
			</td>
			<td>
				Screen Filtering:
				<select id="filtering">
					<option value="nearest">Nearest</option>
					<option value="linear" selected>Linear</option>
				</select>
			</td>
			<!-- <td>
				Screen Sizing:
				<select id="sizing">
					<option value="even">Even</option>
					<option value="top">Emphasize Top</option>
					<option value="bottom">Emphasize Bottom</option>
					<option value="auto">Auto</option>
				</select>
			</td> -->
			<td>
				Swap Screens:
				<input type="checkbox" id="swap" value="swap">
			</td>

			<td>
				Volume:
				<input type="range" min="0" max="256" value="256" id="volume">
			</td>
		</tr>
	</table>
</div>
<div class="emscripten"><progress id="progress" max="1" value="0" hidden=""></progress></div>
<div class="emscripten_border" style="display: flex; justify-content: center;">
	<canvas class="emscripten" id="canvas" oncontextmenu="event.preventDefault()" tabindex="-1" width="650" height="580" style="cursor: default; margin: auto; display: block;"></canvas>
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