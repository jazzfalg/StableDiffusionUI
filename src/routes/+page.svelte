<script>
    import { browser } from "$app/environment";

    let userprompt = "";
    let result = "result_before.png";
    let imgHistory = [];
    let steps = 5;
    let cfgScale = 7;
    let processing = false;
    let percentage = 0;
    let time = "00:00:00";
    let rawprompt = "";
    let medium = "";
    let tags = "";
    let batch = [];
    let currentImgs = [];
    let batchCount = 0;
    let width = 128;
    let height = 128;
    let batchesToGenerate = 1;
    let batchSize = 4;
    let negativePrompt = "";
    let id = 0;
    let selectedMedium = null;
    let selectedButton = null;
    
    // 172.17.11.23:7860



    function handleButtonClick(event) {
        selectedButton = event.target;
    }

    function onKeyDown(e) {
        if (e.key === "Enter" || e.keyCode === 13) {
            e.preventDefault();
            sendPrompt();
        }
    }

    function addMedium(med) {
        medium = med;
    }

    function createTag(tag) {
        if (rawprompt.includes(tag)) {
            const element = document.getElementById(tag);
            element.remove();
            tags = tags.replace(", " + tag, "");
            return;
        }

        let div = document.createElement("div");
        div.id = tag;
        div.classList.add("tag");
        // div.className = "tag";
        div.innerHTML = '<img src="close--filled.png" alt="">' + tag;
        div.style.display = "flex";
        div.style.padding = "4px 6px";
        div.style.alignItems = "center";
        div.style.gap = "6px";
        div.style.color = "#FAFAFA";
        div.style.borderRadius = "3px";
        div.style.background = "#195DE5";
        div.style.fontSize = "11px";
        div.style.fontFamily = "IBM Plex Sans";

        tags += ", " + tag;

        document.getElementById("tag-frame").appendChild(div);
    }

    setInterval(function () {
        rawprompt = medium + userprompt + tags;
    }, 0);

    async function sendPrompt() {
        if (processing) return;

        if (userprompt == "") {
            alert("No Prompt");
            return;
        }

        result = "result_before.png";

        console.log('Sending: "' + rawprompt + '"');

        processing = true;

        let counter = setInterval(function () {
            checkProgress();
            if (!processing) {
                clearInterval(counter);
            }
        }, 200);

        await fetch("http://127.0.0.1:7860/sdapi/v1/txt2img", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Accept: "application/json",
            },
            body: JSON.stringify(buildPrompt()),
        })
            .then((res) => res.json())
            .then((data) => {
                processing = false;
                currentImgs = [];
                var image = new Image();
                image.src = "data:image/png;base64," + data["images"][0];
                // console.log("Image.src" + image.src);
                result = image.src;
                percentage = 100;
                time = "00:00:00";

                batch = [];
                data["images"].forEach((element) => {
                    batch.push("data:image/png;base64," + element);
                    localStorage.setItem(
                        "imgData",
                        "data:image/png;base64," + element
                    );
                });
                imgHistory.push(batch);
                currentImgs.push(batch);

                addToHistory(currentImgs);

                document.getElementById("progressBar").style.width = 100 + "%";
            })
            .catch((err) => console.log(err));
    }

    function buildPrompt() {
        // let rawprompt = {
        //     prompt: userprompt,
        //     steps: steps,
        //     cfg_scale: cfgScale,
        // };

        let payload = {
            enable_hr: false,
            denoising_strength: 0,
            firstphase_width: 0,
            firstphase_height: 0,
            hr_scale: 2,
            hr_second_pass_steps: 0,
            hr_resize_x: 0,
            hr_resize_y: 0,
            prompt: rawprompt,
            seed: -1,
            subseed: -1,
            subseed_strength: 0,
            seed_resize_from_h: -1,
            seed_resize_from_w: -1,
            sampler_name: "LMS",
            batch_size: batchSize,
            n_iter: batchesToGenerate,
            steps: steps,
            cfg_scale: cfgScale,
            width: width,
            height: height,
            restore_faces: false,
            tiling: false,
            do_not_save_samples: false,
            do_not_save_grid: false,
            negative_prompt: "",
            eta: 0,
            s_churn: 0,
            s_tmax: 0,
            s_tmin: 0,
            s_noise: 1,
            override_settings: {},
            override_settings_restore_afterwards: true,
            script_args: [],
            sampler_index: "k_lms",
            send_images: true,
            save_images: false,
            alwayson_scripts: {},
        };
        return payload;
    }

    function checkProgress() {
        if (!processing) return;

        fetch(
            "http://127.0.0.1:7860/sdapi/v1/progress?skip_current_image=false",
            {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json",
                },
            }
        )
            .then((res) => res.json())
            .then((data) => {
                // console.log(data);
                percentage = Math.round(data.progress * 100);
                time = new Date(data.eta_relative * 1000)
                    .toISOString()
                    .slice(11, 19);
                var currentImage = new Image();
                currentImage.src =
                    "data:image/png;base64," + data["current_image"];
                if (data["current_image"] != null) result = currentImage.src;

                document.getElementById("progressBar").style.width =
                    percentage + "%";
            })
            .catch((err) => console.log(err));
    }

    function addToHistory(currentImgs) {
        currentImgs.forEach((batch) => {
            let batchDiv = document.createElement("div");
            batchDiv.id = "batch" + batchCount;
            batchDiv.classList.add("batch");
            batchDiv.style.display = "flex";
            batchDiv.style.padding = "6px";
            batchDiv.style.alignItems = "flex-start";
            // batchDiv.style.alignContent = "flex-start";
            batchDiv.style.gap = "6px";
            batchDiv.style.flexWrap = "wrap";
            batchDiv.style.borderRadius = "10px";
            batchDiv.style.background = "#d1dffa";
            batchDiv.style.boxShadow = "0px 2px 3px 0px rgba(31, 37, 71, 0.25)";

            //             display: flex;
            // padding: 6px;
            // align-items: flex-start;
            // gap: 6px;

            document.getElementById("batches").appendChild(batchDiv);

            batch.forEach((img) => {
                let div = document.createElement("div");
                div.id = "bild";
                div.innerHTML =
                    "<img src=" +
                    img +
                    ' alt="" style="border-radius: 4px; flex-shrink: 0; width: 80px; height: 80px; position: relative;" class="bild" tabindex="1">';
                div.style.borderRadius = "4px";
                div.style.flexShrink = "0";
                div.style.width = "80px";
                div.style.height = "80px";
                div.style.position = "relative";
                div.style.cursor = "pointer";

                document.getElementById("batch" + batchCount).appendChild(div);
            });
            batchCount++;

            // console.log(document.getElementsByClassName("bild")[0].innerHTML);
        });
    }

    if (browser) {
        const batchImagesContainer = document.querySelector(".batches");

        batchImagesContainer.addEventListener("click", (event) => {
            if (event.target instanceof Element) {
                let clickedDiv = event.target;
                if (clickedDiv.getAttribute("src") != null) {
                    console.log(clickedDiv);
                    result = clickedDiv.getAttribute("src");
                }
            }
        });
    }
</script>

<div class="main">
    <div class="history">
        <div class="batches" id="batches">
            <!-- <div class="batch-1" id="batch-1" /> -->
        </div>

        <!-- <div class="scroll-bar">
            <div class="rectangle-8" />

            <div class="rectangle-9" />
        </div> -->

        <div class="history-title">
            <div class="history-shadow" />

            <div class="history-bg" />

            <div class="history2">History</div>
        </div>
    </div>

    <div class="result">
        <div class="result-buttons">
            <div class="button">
                <!-- <div class="button2">Save Image</div> -->

                <a href={result} download="new-image-name.png" class="button2"
                    >Save Image</a
                >
            </div>

            <svg
                class="_16-px-arrow-left"
                width="16"
                height="16"
                viewBox="0 0 16 16"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M6.7 12.3L2.9 8.5H15V7.5H2.9L6.7 3.7L6 3L1 8L6 13L6.7 12.3Z"
                    fill="#195DE6"
                />
            </svg>

            <svg
                class="_16-px-arrow-right"
                width="16"
                height="16"
                viewBox="0 0 16 16"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M9.3 3.7L13.1 7.5H1V8.5H13.1L9.3 12.3L10 13L15 8L10 3L9.3 3.7Z"
                    fill="#195DE6"
                />
            </svg>

            <div class="button-icon">
                <svg
                    class="_16-px-folder"
                    width="16"
                    height="16"
                    viewBox="0 0 16 16"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M5.585 3L7.295 4.705L7.585 5H14V13H2V3H5.585ZM5.585 2H2C1.73478 2 1.48043 2.10536 1.29289 2.29289C1.10536 2.48043 1 2.73478 1 3V13C1 13.2652 1.10536 13.5196 1.29289 13.7071C1.48043 13.8946 1.73478 14 2 14H14C14.2652 14 14.5196 13.8946 14.7071 13.7071C14.8946 13.5196 15 13.2652 15 13V5C15 4.73478 14.8946 4.48043 14.7071 4.29289C14.5196 4.10536 14.2652 4 14 4H8L6.295 2.295C6.20197 2.20142 6.09134 2.12717 5.96948 2.07654C5.84763 2.02591 5.71696 1.9999 5.585 2V2Z"
                        fill="#195DE6"
                    />
                </svg>
            </div>
        </div>

        <div class="result-container">
            <img class="_00013-3684755116-1" src={result} />
        </div>

        <div class="progress">
            <div class="progress-bar">
                <div class="rectangle-38" />

                <div class="rectangle-39" id="progressBar" />
            </div>

            <div class="progress-100">Progress: {percentage}%</div>

            <div class="_0-00-min">{time} ETA</div>
        </div>

        <div class="result3">Result</div>
    </div>

    <div class="buttons">
        <button class="button3" on:click={() => sendPrompt()}>
            Generate
        </button>

        <div class="button4">
            <div class="button5">Save Configuration</div>
        </div>

        <div class="button6">
            <div class="button5">Open Configurations</div>
        </div>
    </div>

    <div class="settings">
        <div class="settings2">Settings</div>

        <div class="resolution">
            <input
                class="input-field"
                type="text"
                placeholder="512"
                bind:value={width}
            />

            <input
                class="input-field2"
                type="text"
                placeholder="512"
                bind:value={height}
            />

            <div class="width">Width</div>

            <div class="hight">Height</div>

            <div class="resolution2">Resolution</div>
        </div>

        <div class="divider" />

        <div class="model">
            <div class="drop-down">
                <div class="drop-down2">v1-5-pruned-ckpt</div>

                <svg
                    class="_8-px-caret-sort"
                    width="8"
                    height="8"
                    viewBox="0 0 8 8"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path d="M6 6L4 8L2 6H6ZM2 2L4 0L6 2H2Z" fill="#47536B" />
                </svg>
            </div>

            <div class="model2">Model</div>

            <svg
                class="_12-px-help"
                width="12"
                height="12"
                viewBox="0 0 12 12"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M6 0.75C3.075 0.75 0.75 3.075 0.75 6C0.75 8.925 3.075 11.25 6 11.25C8.925 11.25 11.25 8.925 11.25 6C11.25 3.075 8.925 0.75 6 0.75ZM6 10.5C3.525 10.5 1.5 8.475 1.5 6C1.5 3.525 3.525 1.5 6 1.5C8.475 1.5 10.5 3.525 10.5 6C10.5 8.475 8.475 10.5 6 10.5Z"
                    fill="#47536B"
                />
                <path
                    d="M6.00039 9.45C6.33176 9.45 6.60039 9.18137 6.60039 8.85C6.60039 8.51863 6.33176 8.25 6.00039 8.25C5.66902 8.25 5.40039 8.51863 5.40039 8.85C5.40039 9.18137 5.66902 9.45 6.00039 9.45Z"
                    fill="#47536B"
                />
                <path
                    d="M6.375 3H5.85C4.875 3 4.125 3.75 4.125 4.65V4.8H4.875V4.65C4.875 4.2 5.325 3.75 5.85 3.75H6.45C6.975 3.75 7.35 4.2 7.35 4.65C7.35 5.1 6.9 5.625 6.375 5.625H5.625V7.275H6.375V6.375C7.275 6.375 8.025 5.625 8.025 4.725C8.025 3.825 7.275 3 6.375 3Z"
                    fill="#47536B"
                />
            </svg>
        </div>

        <div class="sampler">
            <div class="drop-down">
                <div class="drop-down2">Euler a</div>

                <svg
                    class="_8-px-caret-sort2"
                    width="8"
                    height="8"
                    viewBox="0 0 8 8"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path d="M6 6L4 8L2 6H6ZM2 2L4 0L6 2H2Z" fill="#47536B" />
                </svg>
            </div>

            <div class="sampler2">Sampler</div>

            <svg
                class="_12-px-help2"
                width="12"
                height="12"
                viewBox="0 0 12 12"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M6 0.75C3.075 0.75 0.75 3.075 0.75 6C0.75 8.925 3.075 11.25 6 11.25C8.925 11.25 11.25 8.925 11.25 6C11.25 3.075 8.925 0.75 6 0.75ZM6 10.5C3.525 10.5 1.5 8.475 1.5 6C1.5 3.525 3.525 1.5 6 1.5C8.475 1.5 10.5 3.525 10.5 6C10.5 8.475 8.475 10.5 6 10.5Z"
                    fill="#47536B"
                />
                <path
                    d="M6.00039 9.45C6.33176 9.45 6.60039 9.18137 6.60039 8.85C6.60039 8.51863 6.33176 8.25 6.00039 8.25C5.66902 8.25 5.40039 8.51863 5.40039 8.85C5.40039 9.18137 5.66902 9.45 6.00039 9.45Z"
                    fill="#47536B"
                />
                <path
                    d="M6.375 3H5.85C4.875 3 4.125 3.75 4.125 4.65V4.8H4.875V4.65C4.875 4.2 5.325 3.75 5.85 3.75H6.45C6.975 3.75 7.35 4.2 7.35 4.65C7.35 5.1 6.9 5.625 6.375 5.625H5.625V7.275H6.375V6.375C7.275 6.375 8.025 5.625 8.025 4.725C8.025 3.825 7.275 3 6.375 3Z"
                    fill="#47536B"
                />
            </svg>
        </div>

        <div class="seed">
            <div class="input-field3">
                <div class="input2">random</div>
            </div>

            <div class="seed2">Seed</div>

            <svg
                class="_12-px-help3"
                width="12"
                height="12"
                viewBox="0 0 12 12"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M6 0.75C3.075 0.75 0.75 3.075 0.75 6C0.75 8.925 3.075 11.25 6 11.25C8.925 11.25 11.25 8.925 11.25 6C11.25 3.075 8.925 0.75 6 0.75ZM6 10.5C3.525 10.5 1.5 8.475 1.5 6C1.5 3.525 3.525 1.5 6 1.5C8.475 1.5 10.5 3.525 10.5 6C10.5 8.475 8.475 10.5 6 10.5Z"
                    fill="#47536B"
                />
                <path
                    d="M6.00039 9.45C6.33176 9.45 6.60039 9.18137 6.60039 8.85C6.60039 8.51863 6.33176 8.25 6.00039 8.25C5.66902 8.25 5.40039 8.51863 5.40039 8.85C5.40039 9.18137 5.66902 9.45 6.00039 9.45Z"
                    fill="#47536B"
                />
                <path
                    d="M6.375 3H5.85C4.875 3 4.125 3.75 4.125 4.65V4.8H4.875V4.65C4.875 4.2 5.325 3.75 5.85 3.75H6.45C6.975 3.75 7.35 4.2 7.35 4.65C7.35 5.1 6.9 5.625 6.375 5.625H5.625V7.275H6.375V6.375C7.275 6.375 8.025 5.625 8.025 4.725C8.025 3.825 7.275 3 6.375 3Z"
                    fill="#47536B"
                />
            </svg>
        </div>

        <div class="guidance-scale">
            <div class="guidance-scale2">Guidance Scale</div>

            <div class="input-field4">
                <div class="input">{cfgScale}</div>
            </div>

            <div class="slider">
                <!-- <div class="rectangle-25" /> -->
                <input
                    type="range"
                    min="1"
                    max="15"
                    bind:value={cfgScale}
                    class="slider2"
                    id="CFGRange"
                />

                <!-- <div class="ellipse-12" /> -->
            </div>

            <svg
                class="_12-px-help4"
                width="12"
                height="12"
                viewBox="0 0 12 12"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M6 0.75C3.075 0.75 0.75 3.075 0.75 6C0.75 8.925 3.075 11.25 6 11.25C8.925 11.25 11.25 8.925 11.25 6C11.25 3.075 8.925 0.75 6 0.75ZM6 10.5C3.525 10.5 1.5 8.475 1.5 6C1.5 3.525 3.525 1.5 6 1.5C8.475 1.5 10.5 3.525 10.5 6C10.5 8.475 8.475 10.5 6 10.5Z"
                    fill="#47536B"
                />
                <path
                    d="M6.00039 9.45C6.33176 9.45 6.60039 9.18137 6.60039 8.85C6.60039 8.51863 6.33176 8.25 6.00039 8.25C5.66902 8.25 5.40039 8.51863 5.40039 8.85C5.40039 9.18137 5.66902 9.45 6.00039 9.45Z"
                    fill="#47536B"
                />
                <path
                    d="M6.375 3H5.85C4.875 3 4.125 3.75 4.125 4.65V4.8H4.875V4.65C4.875 4.2 5.325 3.75 5.85 3.75H6.45C6.975 3.75 7.35 4.2 7.35 4.65C7.35 5.1 6.9 5.625 6.375 5.625H5.625V7.275H6.375V6.375C7.275 6.375 8.025 5.625 8.025 4.725C8.025 3.825 7.275 3 6.375 3Z"
                    fill="#47536B"
                />
            </svg>
        </div>

        <div class="steps">
            <div class="steps2">Steps</div>

            <div class="input-field4">
                <div class="input">{steps}</div>
            </div>

            <div class="slider">
                <input
                    type="range"
                    id="stepSlider"
                    class="slider2"
                    min="1"
                    max="50"
                    step="1"
                    bind:value={steps}
                />
            </div>

            <svg
                class="_12-px-help5"
                width="12"
                height="12"
                viewBox="0 0 12 12"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M6 0.75C3.075 0.75 0.75 3.075 0.75 6C0.75 8.925 3.075 11.25 6 11.25C8.925 11.25 11.25 8.925 11.25 6C11.25 3.075 8.925 0.75 6 0.75ZM6 10.5C3.525 10.5 1.5 8.475 1.5 6C1.5 3.525 3.525 1.5 6 1.5C8.475 1.5 10.5 3.525 10.5 6C10.5 8.475 8.475 10.5 6 10.5Z"
                    fill="#47536B"
                />
                <path
                    d="M6.00039 9.45C6.33176 9.45 6.60039 9.18137 6.60039 8.85C6.60039 8.51863 6.33176 8.25 6.00039 8.25C5.66902 8.25 5.40039 8.51863 5.40039 8.85C5.40039 9.18137 5.66902 9.45 6.00039 9.45Z"
                    fill="#47536B"
                />
                <path
                    d="M6.375 3H5.85C4.875 3 4.125 3.75 4.125 4.65V4.8H4.875V4.65C4.875 4.2 5.325 3.75 5.85 3.75H6.45C6.975 3.75 7.35 4.2 7.35 4.65C7.35 5.1 6.9 5.625 6.375 5.625H5.625V7.275H6.375V6.375C7.275 6.375 8.025 5.625 8.025 4.725C8.025 3.825 7.275 3 6.375 3Z"
                    fill="#47536B"
                />
            </svg>
        </div>

        <div class="batch">
            <!-- <div class="input-field">4</div> -->

            <input
                class="input-field"
                type="text"
                bind:value={batchesToGenerate}
            />

            <input class="input-field2" type="text" bind:value={batchSize} />

            <!-- <div class="input-field2">
                <div class="input">1</div>
            </div> -->

            <div class="count">Count</div>

            <div class="size">Size</div>

            <div class="batch2">Batch</div>

            <svg
                class="_12-px-help6"
                width="12"
                height="12"
                viewBox="0 0 12 12"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M6 0.75C3.075 0.75 0.75 3.075 0.75 6C0.75 8.925 3.075 11.25 6 11.25C8.925 11.25 11.25 8.925 11.25 6C11.25 3.075 8.925 0.75 6 0.75ZM6 10.5C3.525 10.5 1.5 8.475 1.5 6C1.5 3.525 3.525 1.5 6 1.5C8.475 1.5 10.5 3.525 10.5 6C10.5 8.475 8.475 10.5 6 10.5Z"
                    fill="#47536B"
                />
                <path
                    d="M6.00039 9.45C6.33176 9.45 6.60039 9.18137 6.60039 8.85C6.60039 8.51863 6.33176 8.25 6.00039 8.25C5.66902 8.25 5.40039 8.51863 5.40039 8.85C5.40039 9.18137 5.66902 9.45 6.00039 9.45Z"
                    fill="#47536B"
                />
                <path
                    d="M6.375 3H5.85C4.875 3 4.125 3.75 4.125 4.65V4.8H4.875V4.65C4.875 4.2 5.325 3.75 5.85 3.75H6.45C6.975 3.75 7.35 4.2 7.35 4.65C7.35 5.1 6.9 5.625 6.375 5.625H5.625V7.275H6.375V6.375C7.275 6.375 8.025 5.625 8.025 4.725C8.025 3.825 7.275 3 6.375 3Z"
                    fill="#47536B"
                />
            </svg>
        </div>
    </div>

    <div class="prompt">
        <div class="raw-prompt">
            <svg
                class="_16-px-chevron-down"
                width="16"
                height="16"
                viewBox="0 0 16 16"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M8 4.99995L13 9.99995L12.3 10.7L8 6.39995L3.7 10.7L3 9.99995L8 4.99995Z"
                    fill="#47536B"
                />
            </svg>

            <div class="rawprompt-frame">
                <div class="negative-prompt-text">
                    {rawprompt}
                </div>
            </div>

            <div class="raw-prompt2">Raw Prompt</div>
        </div>

        <div class="tags">
            <div class="tag-frame" id="tag-frame" />

            <div class="tags2">Tags</div>
        </div>

        <div class="negative-content">
           
                <textarea
                    bind:value={negativePrompt}
                    class="content-frame"
                />
         

            <div class="negative-content2">Negative Content</div>
        </div>

        <div class="content">
            <div class="content2">Content</div>

            <!-- <input
                bind:value={userprompt}
                on:keydown={onKeyDown}
                class="content-frame"
            /> -->

            <textarea
                bind:value={userprompt}
                on:keydown={onKeyDown}
                class="content-frame"
            />
        </div>
        <div class="prompt2">Prompt</div>
    </div>

    <div class="tags3">
        <div class="tag-container">
            <div class="styles">
                <div class="styles2">Styles</div>

                <div class="optional">optional</div>

                <div class="drop-down3">
                    <div class="drop-down2">most relevant</div>

                    <svg
                        class="_8-px-caret-sort3"
                        width="8"
                        height="8"
                        viewBox="0 0 8 8"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                    >
                        <path
                            d="M6 6L4 8L2 6H6ZM2 2L4 0L6 2H2Z"
                            fill="#47536B"
                        />
                    </svg>
                </div>

                <div class="frame-18">
                    <div class="chip">
                        <div class="chip2">Effects</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Materials</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Concepts</div>
                    </div>

                    <div class="chip3">
                        <div class="chip4">Movements</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Other</div>
                    </div>
                </div>

                <div class="divider2" />

                <div class="styles-kacheln">
                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-82" src="rectangle-82.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Cubism</div>
                    </div>

                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-82" src="rectangle-82.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox2"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Surrealism</div>
                    </div>

                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-82" src="rectangle-82.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox3"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Renaissance</div>
                    </div>

                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-82" src="rectangle-82.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox4"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Pop Art</div>
                    </div>

                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-7" src="rectangle-7.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox5"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Descriptions</div>
                    </div>

                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-7" src="rectangle-7.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox6"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Descriptions</div>
                    </div>

                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-7" src="rectangle-7.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox7"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Descriptions</div>
                    </div>

                    <div class="kachel">
                        <div class="vorschau">
                            <img class="rectangle-7" src="rectangle-7.png" />
                        </div>

                        <svg
                            class="_10-px-checkbox8"
                            width="10"
                            height="11"
                            viewBox="0 0 10 11"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>

                        <div class="photography">Descriptions</div>
                    </div>
                </div>
            </div>

            <div class="techniques">
                <div class="techniques2">Techniques</div>

                <div class="optional2">optional</div>

                <div class="drop-down3">
                    <div class="drop-down2">most relevant</div>

                    <svg
                        class="_8-px-caret-sort4"
                        width="8"
                        height="8"
                        viewBox="0 0 8 8"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                    >
                        <path
                            d="M6 6L4 8L2 6H6ZM2 2L4 0L6 2H2Z"
                            fill="#47536B"
                        />
                    </svg>
                </div>

                <div class="techniques-tabs">
                    <div class="chip">
                        <div class="chip2">Photography</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Rendering</div>
                    </div>

                    <div class="chip3">
                        <div class="chip4">Painting</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Drawing</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Graphic</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Print</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Digital Art</div>
                    </div>

                    <div class="chip">
                        <div class="chip2">Other</div>
                    </div>
                </div>

                <div class="show-more">
                    <svg
                        class="_10-px-chevron-down"
                        width="10"
                        height="10"
                        viewBox="0 0 10 10"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                    >
                        <path
                            d="M5 6.875L1.875 3.75L2.3125 3.3125L5 6L7.6875 3.3125L8.125 3.75L5 6.875Z"
                            fill="#195DE6"
                        />
                    </svg>

                    <div class="show-more2">show more</div>
                </div>

                <div class="divider3" />

                <div class="techniques-kacheln">
                    <a href="" on:click={() => createTag("Oil Painting")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox9"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Oil</div>
                        </div>
                    </a>

                    <a href="" on:click={() => createTag("Ink Painting")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox10"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Ink</div>
                        </div>
                    </a>

                    <a href="" on:click={() => createTag("Brush work")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox11"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Brush</div>
                        </div>
                    </a>

                    <a href="" on:click={() => createTag("Graffiti")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-72"
                                    src="rectangle-72.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox12"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Graffiti</div>
                        </div>
                    </a>

                    <a
                        href=""
                        on:click={() => createTag("in the style of Gouache")}
                    >
                        <div class="kachel">
                            <div class="vorschau">
                                <img class="airbrush" src="airbrush.png" />
                            </div>

                            <svg
                                class="_10-px-checkbox13"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Gouache</div>
                        </div>
                    </a>

                    <a
                        href=""
                        on:click={() => createTag("Watercolor painting")}
                    >
                        <div class="kachel2">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <div class="text">
                                <svg
                                    class="_10-px-checkbox-checked-filled"
                                    width="10"
                                    height="10"
                                    viewBox="0 0 10 10"
                                    fill="none"
                                    xmlns="http://www.w3.org/2000/svg"
                                >
                                    <path
                                        d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM4.375 6.71875L2.8125 5.16959L3.30963 4.6875L4.375 5.733L6.69022 3.4375L7.18766 3.93037L4.375 6.71875Z"
                                        fill="#195DE6"
                                    />
                                </svg>

                                <div class="photography2">Watercolor</div>
                            </div>
                        </div>
                    </a>

                    <a href="" on:click={() => createTag("Airbrush work")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img class="gouache" src="gouache.png" />
                            </div>

                            <svg
                                class="_10-px-checkbox14"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Airbrush</div>
                        </div>
                    </a>

                    <a
                        href=""
                        on:click={() => createTag("Alla Prima painting")}
                    >
                        <div class="kachel2">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <div class="text">
                                <svg
                                    class="_10-px-checkbox-checked-filled2"
                                    width="10"
                                    height="10"
                                    viewBox="0 0 10 10"
                                    fill="none"
                                    xmlns="http://www.w3.org/2000/svg"
                                >
                                    <path
                                        d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM4.375 6.71875L2.8125 5.16959L3.30963 4.6875L4.375 5.733L6.69022 3.4375L7.18766 3.93037L4.375 6.71875Z"
                                        fill="#195DE6"
                                    />
                                </svg>

                                <div class="photography2">Alla Prima</div>
                            </div>
                        </div>
                    </a>

                    <a
                        href=""
                        on:click={() => createTag("Pointilism painting")}
                    >
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox15"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Pointilism</div>
                        </div>
                    </a>

                    <a href="" on:click={() => createTag("Impasto painting")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox16"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Impasto</div>
                        </div>
                    </a>

                    <a href="" on:click={() => createTag("Stippling work")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-72"
                                    src="rectangle-72.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox17"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Stippling</div>
                        </div>
                    </a>

                    <a href="" on:click={() => createTag("Chiaroscuro style")}>
                        <div class="kachel">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <svg
                                class="_10-px-checkbox18"
                                width="10"
                                height="11"
                                viewBox="0 0 10 11"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M8.125 1.75H1.875C1.70924 1.75 1.55027 1.81585 1.43306 1.93306C1.31585 2.05027 1.25 2.20924 1.25 2.375V8.625C1.25 8.79076 1.31585 8.94973 1.43306 9.06694C1.55027 9.18415 1.70924 9.25 1.875 9.25H8.125C8.29076 9.25 8.44973 9.18415 8.56694 9.06694C8.68415 8.94973 8.75 8.79076 8.75 8.625V2.375C8.75 2.20924 8.68415 2.05027 8.56694 1.93306C8.44973 1.81585 8.29076 1.75 8.125 1.75V1.75ZM1.875 8.625V2.375H8.125V8.625H1.875Z"
                                    fill="#195DE6"
                                />
                            </svg>

                            <div class="photography">Chiaroscuro</div>
                        </div>
                    </a>
                </div>
            </div>

            <div class="medium">
                <div class="medium2">Medium</div>

                <div class="medium-frame">
                    <a href="" on:click={() => addMedium("A photograph of ")}>
                        <div class="kachel3">
                            <div class="vorschau">
                                <img
                                    class="rectangle-7"
                                    src="rectangle-7.png"
                                />
                            </div>
                            <div class="photography3">Photography</div>
                        </div>
                    </a>

                    <a href="" on:click={() => addMedium("A rendering of ")}>
                        <div class="kachel3">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <div class="photography3">Rendering</div>
                        </div>
                    </a>

                    <a href="" on:click={() => addMedium("A painting of ")}>
                        <div class="kachel3">
                            <div class="vorschau">
                                <img
                                    class="rectangle-72"
                                    src="rectangle-72.png"
                                />
                            </div>

                            <div class="photography4">Painting</div>
                        </div>
                    </a>

                    <a href="" on:click={() => addMedium("A drawing of ")}>
                        <div class="kachel3">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <div class="photography3">Drawing</div>
                        </div>
                    </a>

                    <a href="" on:click={() => addMedium("A graphic of ")}>
                        <div class="kachel3">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <div class="photography3">Graphic</div>
                        </div>
                    </a>

                    <a href="" on:click={() => addMedium("A print of ")}>
                        <div class="kachel3">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <div class="photography3">Print</div>
                        </div>
                    </a>

                    <a
                        href=""
                        on:click={() => addMedium("A digital artwork of ")}
                    >
                        <div class="kachel3">
                            <div class="vorschau">
                                <img
                                    class="rectangle-82"
                                    src="rectangle-82.png"
                                />
                            </div>

                            <div class="photography3">Digital Art</div>
                        </div>
                    </a>

                    <div class="kachel5">
                        <div class="vorschau">
                            <img class="rectangle-73" src="rectangle-73.png" />
                        </div>

                        <div class="new-idea">New Idea</div>

                        <svg
                            class="_32-px-add-filled"
                            width="32"
                            height="32"
                            viewBox="0 0 32 32"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M16 2C12.3009 2.04476 8.76586 3.53412 6.14999 6.14999C3.53412 8.76586 2.04476 12.3009 2 16C2.04476 19.6991 3.53412 23.2341 6.14999 25.85C8.76586 28.4659 12.3009 29.9552 16 30C19.6991 29.9552 23.2341 28.4659 25.85 25.85C28.4659 23.2341 29.9552 19.6991 30 16C29.9552 12.3009 28.4659 8.76586 25.85 6.14999C23.2341 3.53412 19.6991 2.04476 16 2V2ZM24 17H17V24H15V17H8V15H15V8H17V15H24V17Z"
                                fill="#FAFAFA"
                            />
                        </svg>
                    </div>
                </div>

                <div class="medium-devider" />
            </div>
        </div>

        <!-- <div class="scroll-bar2">
            <div class="rectangle-83" />

            <div class="rectangle-92" />
        </div> -->

        <div class="search">
            <div class="search2">search...</div>

            <svg
                class="_8-px-search"
                width="10"
                height="10"
                viewBox="0 0 10 10"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M9.37525 8.93751L6.68775 6.25001C7.87525 4.81251 7.68775 2.62501 6.25025 1.43751C4.81275 0.250009 2.62525 0.437509 1.43775 1.87501C0.250253 3.31251 0.437753 5.50001 1.87525 6.68751C3.12525 7.75001 5.00025 7.75001 6.25025 6.68751L8.93775 9.37501L9.37525 8.93751ZM1.25025 4.06251C1.25025 2.50001 2.50025 1.25001 4.06275 1.25001C5.62525 1.25001 6.87525 2.50001 6.87525 4.06251C6.87525 5.62501 5.62525 6.87501 4.06275 6.87501C2.50025 6.87501 1.25025 5.62501 1.25025 4.06251Z"
                    fill="#47536B"
                />
            </svg>
        </div>

        <div class="fade" />

        <div class="tags4">Tags</div>
    </div>

    <div class="top-bar">
        <div class="green-ellipse" />

        <div class="yellow-ellipse" />

        <div class="red-ellipse" />

        <div class="projects">
            <span
                ><span class="projects-span">Projects / </span><span
                    class="projects-span2">Untitled Project</span
                ></span
            >
        </div>
    </div>
</div>

<style>
    .main,
    .main * {
        box-sizing: border-box;
    }

    .main {
        background: var(--bluebackground, #edeef3);
        border-radius: 12px;
        width: 1920px;
        height: 1080px;
        position: relative;
        overflow: hidden;
    }

    .history {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 407px;
        height: 996px;
        position: absolute;
        left: 1493px;
        top: 64px;
        box-shadow: var(
            --shadow-section-2-box-shadow,
            0px 4px 10px 0px rgba(31, 37, 71, 0.15)
        );
        overflow: hidden;
    }

    .batches {
        /* padding: 13px 0px 13px 0px; */
        display: flex;

        gap: 16px;
        align-items: flex-start;
        position: absolute;
        left: 20px;
        top: 102px;
        display: inline-flex;
        height: 874px;

        align-items: flex-start;
        gap: 16px;
        flex-shrink: 0;
        display: inline-flex;
        height: 874px;
        /* padding: 16px 0px; */

        align-items: flex-start;
        gap: 16px;
        flex-shrink: 0;

        display: flex;
        width: 372px;
        padding: 13px 22px 13px 0px;
        align-items: flex-start;
        align-content: flex-start;
        gap: 16px;
        flex-wrap: wrap;
        overflow-y: scroll;
    }

    .batch-1 {
        display: flex;
        width: 350px;
        padding: 6px;
        align-items: flex-start;
        align-content: flex-start;
        gap: 6px;
        flex-wrap: wrap;
        border-radius: 10px;
        background: var(--blue-accent-light, #d1dffa);

        /* shadow_kachel */
        box-shadow: 0px 2px 3px 0px rgba(31, 37, 71, 0.25);
    }

    ._00009-1938723002 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }

    ._00014-446255131 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }

    ._00012-3684755115 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }

    ._00013-3684755116-6 {
        border-radius: 4px;
        border-style: solid;
        border-color: var(--blueaccent, #195de5);
        border-width: 2px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }

    .scroll-bar {
        width: 7px;
        height: 874px;
        position: absolute;
        left: 392px;
        top: 976px;
        transform-origin: 0 0;
        transform: rotate(-180deg) scale(1, 1);
    }

    .rectangle-8 {
        background: var(--bluebackground, #edeef3);
        border-radius: 100px;
        width: 7px;
        height: 874px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .rectangle-9 {
        background: var(--bluemid, #94a0b8);
        border-radius: 100px;
        width: 7px;
        height: 290.68px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .history-title {
        background: var(--white, #fafafa);
        width: 407px;
        height: 101px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .history-shadow {
        background: var(--white, #fafafa);
        border-style: solid;
        border-color: var(--bluebackground, #edeef3);
        border-width: 0px 0px 2px 0px;
        width: 360px;
        height: 102px;
        position: absolute;
        left: 15px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }

    .history-bg {
        background: var(--white, #fafafa);
        width: 407px;
        height: 101px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .history2 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 20px;
        top: 20px;
    }

    .result {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 646px;
        height: 996px;
        position: absolute;
        left: 827px;
        top: 64px;
        box-shadow: var(
            --shadow-section-2-box-shadow,
            0px 4px 10px 0px rgba(31, 37, 71, 0.15)
        );
    }

    ._00013-3684755116-1 {
        flex-shrink: 0;
        width: 606px;
        height: 606px;
        position: relative;
    }

    .result-container,
    .result-container * {
        box-sizing: border-box;
    }
    .result-container {
        background: var(--white, #fafafa);
        border-radius: 4px;
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: center;
        justify-content: center;

        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
        position: absolute;
        left: 20px;
        top: 102px;
    }

    .result-buttons {
        width: 606px;
        height: 30px;
        position: absolute;
        left: 20px;
        top: 946px;
    }

    .button {
        background: var(--blueaccent, #195de5);
        border-radius: 4px;
        padding: 8px 12px 8px 12px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: center;
        width: 155px;
        position: absolute;
        left: 451px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }

    .button2 {
        color: var(--white, #fafafa);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        cursor: pointer;
    }

    ._16-px-arrow-left {
        position: absolute;
        left: 0px;
        top: 8px;
        overflow: visible;
    }

    ._16-px-arrow-right {
        position: absolute;
        left: 26px;
        top: 8px;
        overflow: visible;
    }

    .button-icon {
        background: var(--blueaccent-light, #d1dffa);
        border-radius: 4px;
        width: 30px;
        height: 30px;
        position: absolute;
        left: 405px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }

    ._16-px-folder {
        position: absolute;
        left: 7px;
        top: 7px;
        overflow: visible;
    }

    .result2 {
        background: var(--bluebackground, #edeef3);
        border-radius: 4px;
        width: 606px;
        height: 606px;
        position: absolute;
        left: 20px;
        top: 102px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }

    .image-1 {
        position: absolute;
        left: 281px;
        top: 281px;
        overflow: visible;
    }

    .progress {
        width: 606px;
        height: 27px;
        position: absolute;
        left: 20px;
        top: 728px;
    }

    .progress-bar {
        background: var(--bluebackground, #edeef3);
        border-radius: 4px;
        width: 606px;
        height: 7px;
        position: absolute;
        left: 0px;
        top: 0px;
        overflow: hidden;
    }

    .rectangle-38 {
        background: var(--bluebackground, #edeef3);
        width: 606px;
        height: 7px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .rectangle-39 {
        background: var(--blue-heading, #195de5);
        border-radius: 100px;
        width: var(--percentage, 0);
        height: 7px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .progress-100 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 13px;
        width: 114px;
        height: 14px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    ._0-00-min {
        color: var(--bluetext, #47536b);
        text-align: right;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 492px;
        top: 13px;
        width: 114px;
        height: 14px;
        display: flex;
        align-items: center;
        justify-content: flex-end;
    }

    .result3 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 20px;
        top: 20px;
    }

    .buttons {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 757px;
        height: 70px;
        position: absolute;
        left: 20px;
        top: 990px;
        box-shadow: var(
            --shadow-section-2-box-shadow,
            0px 4px 10px 0px rgba(31, 37, 71, 0.15)
        );
        overflow: hidden;
    }

    :global(.button3) {
        color: var(--white, #fafafa);
        background: var(--blueaccent, #195de5);
        border-radius: 4px;
        padding: 8px 12px 8px 12px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: center;
        width: 290px;
        position: absolute;
        left: 447px;
        top: 20px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        border: none;
        cursor: pointer;
    }

    .button4 {
        background: var(--blueaccent-light, #d1dffa);
        border-radius: 4px;
        padding: 8px 12px 8px 12px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: center;
        width: 176px;
        position: absolute;
        left: 20px;
        top: 20px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }

    .button5 {
        color: var(--blueaccent, #195de5);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .button6 {
        background: var(--blueaccent-light, #d1dffa);
        border-radius: 4px;
        padding: 8px 12px 8px 12px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: center;
        width: 176px;
        position: absolute;
        left: 216px;
        top: 20px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }

    .settings {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 330px;
        height: 377px;
        position: absolute;
        left: 447px;
        top: 593px;
        box-shadow: var(
            --shadow-section-2-box-shadow,
            0px 4px 10px 0px rgba(31, 37, 71, 0.15)
        );
        overflow: hidden;
    }

    .settings2 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 20px;
        top: 20px;
    }

    .resolution {
        width: 126px;
        height: 72px;
        position: absolute;
        left: 20px;
        top: 285px;
    }

    .input-field {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 90px;
        position: absolute;
        left: 0px;
        top: 22px;
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
    }

    .input {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .input-field2 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 90px;
        position: absolute;
        left: 0px;
        top: 50px;
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
    }

    .width {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 96px;
        top: 26px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .hight {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 96px;
        top: 54px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .resolution2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .divider {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 290px;
        height: 2px;
        position: absolute;
        left: 20px;
        top: 263px;
    }

    .model {
        width: 135px;
        height: 44px;
        position: absolute;
        left: 20px;
        top: 71px;
    }

    .drop-down {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        width: 135px;
        position: absolute;
        left: 0px;
        top: 22px;
    }

    .drop-down2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    ._8-px-caret-sort {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .model2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._12-px-help {
        position: absolute;
        left: 117px;
        top: 1px;
        overflow: visible;
    }

    .sampler {
        width: 135px;
        height: 44px;
        position: absolute;
        left: 175px;
        top: 71px;
    }

    ._8-px-caret-sort2 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .sampler2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._12-px-help2 {
        position: absolute;
        left: 117px;
        top: 1px;
        overflow: visible;
    }

    .seed {
        width: 135px;
        height: 44px;
        position: absolute;
        left: 20px;
        top: 199px;
    }

    .input-field3 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 135px;
        position: absolute;
        left: 0px;
        top: 22px;
    }

    .input2 {
        color: var(--bluemid, #94a0b8);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .seed2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._12-px-help3 {
        position: absolute;
        left: 117px;
        top: 1px;
        overflow: visible;
    }

    .guidance-scale {
        width: 135px;
        height: 44px;
        position: absolute;
        left: 20px;
        top: 135px;
    }

    .guidance-scale2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .input-field4 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 29px;
        position: absolute;
        left: 106px;
        top: 22px;
    }

    .slider {
        width: 100px;
        height: 8px;
        position: absolute;
        left: 0px;
        top: 27px;
    }

    .slider2 {
        -webkit-appearance: none; /* Override default CSS styles */
        position: absolute;
        appearance: none;
        width: 100%; /* Full-width */
        height: 4px; /* Specified height */
        background: #d3d3d3; /* Grey background */
        outline: none; /* Remove outline */
        opacity: 0.7; /* Set transparency (for mouse-over effects on hover) */
        -webkit-transition: 0.2s; /* 0.2 seconds transition on hover */
        transition: opacity 0.2s;
    }

    /* Mouse-over effects */
    .slider2:hover {
        opacity: 1; /* Fully shown on mouse-over */
    }

    .slider2::-webkit-slider-thumb {
        -webkit-appearance: none; /* Override default look */
        appearance: none;
        width: 9px; /* Set a specific slider handle width */
        height: 9px; /* Slider handle height */
        border-radius: 50%;
        background: var(--bluetext, #47536b); /* Green background */
        cursor: pointer; /* Cursor on hover */
    }

    .slider2::-moz-range-thumb {
        width: 25px; /* Set a specific slider handle width */
        height: 25px; /* Slider handle height */
        background: var(--bluetext, #47536b); /* Green background */
        border-radius: 50%;
        cursor: pointer; /* Cursor on hover */
    }

    .rectangle-25 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 100px;
        height: 4px;
        position: absolute;
        left: 0px;
        top: 2px;
    }

    .ellipse-12 {
        background: var(--bluemid, #94a0b8);
        border-radius: 50%;
        width: 8px;
        height: 8px;
        position: absolute;
        left: calc(50% - 6px);
        top: calc(50% - 4px);
    }

    ._12-px-help4 {
        position: absolute;
        left: 117px;
        top: 1px;
        overflow: visible;
    }

    .steps {
        width: 135px;
        height: 44px;
        position: absolute;
        left: 175px;
        top: 135px;
    }

    .steps2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._12-px-help5 {
        position: absolute;
        left: 117px;
        top: 1px;
        overflow: visible;
    }

    .batch {
        width: 129px;
        height: 72px;
        position: absolute;
        left: 175px;
        top: 285px;
    }

    .count {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 96px;
        top: 26px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .size {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 96px;
        top: 54px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .batch2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._12-px-help6 {
        position: absolute;
        left: 117px;
        top: 1px;
        overflow: visible;
    }

    .prompt {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 330px;
        height: 509px;
        position: absolute;
        left: 447px;
        top: 64px;
        box-shadow: var(
            --shadow-section-2-box-shadow,
            0px 4px 10px 0px rgba(31, 37, 71, 0.15)
        );
        overflow: hidden;
    }

    .raw-prompt {
        width: 290px;
        height: 97px;
        position: absolute;
        left: 20px;
        top: 392px;
    }

    ._16-px-chevron-down {
        position: absolute;
        left: 290px;
        top: 20px;
        transform: translate(-16px, -16px);
        overflow: visible;
    }

    .rawprompt-frame {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 7px 9px 7px 9px;
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 290px;
        height: 66px;
        position: absolute;
        left: 0px;
        top: 31px;
        overflow-y: scroll;
    }

    .negative-prompt-text {
        text-align: left;
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
        position: relative;
        flex: 1;
    }

    .negative-prompt-text-span {
        color: var(--blueaccent, #195de5);
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
    }

    .negative-prompt-text-span2 {
        color: var(--bluetext, #47536b);
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
    }

    .negative-prompt-text-span3 {
        color: var(--blueaccent, #195de5);
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
    }

    .raw-prompt2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .tags {
        width: 290px;
        height: 97px;
        position: absolute;
        left: 20px;
        top: 285px;
    }

    .tag-frame {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        padding: 8px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 290px;
        height: 66px;
        position: absolute;
        left: 0px;
        top: 31px;
        overflow-y: scroll;
    }

    .tag {
        background: var(--blueaccent, #195de5);
        border-radius: 3px;
        padding: 4px 6px 4px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: center;
        justify-content: flex-start;
        flex-shrink: 0;
        position: relative;
    }

    .frame-19 {
        display: flex;
        flex-direction: row;
        gap: 4px;
        align-items: center;
        justify-content: flex-start;
        flex-shrink: 0;
        position: relative;
    }

    ._8-px-close-filled {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .tag2 {
        color: var(--white, #fafafa);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .frame-20 {
        display: flex;
        flex-direction: row;
        gap: 2px;
        align-items: center;
        justify-content: flex-start;
        flex-shrink: 0;
        position: relative;
    }

    ._8-px-subtract {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._1-0 {
        color: var(--white, #fafafa);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    ._8-px-add {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._8-px-close-filled2 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._8-px-subtract2 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._8-px-add2 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._8-px-close-filled3 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._8-px-subtract3 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._8-px-add3 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .tags2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .negative-content {
        width: 290px;
        height: 97px;
        position: absolute;
        left: 20px;
        top: 178px;
    }

    .negative-content-frame {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 7px 9px 7px 9px;
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 290px;
        height: 66px;
        position: absolute;
        left: 0px;
        top: 31px;
        overflow: hidden;
    }

    .negative-prompt-text2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        align-self: stretch;
        flex: 1;
    }

    .negative-content2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .content {
        width: 290px;
        height: 97px;
        position: absolute;
        left: 20px;
        top: 71px;
    }

    .content2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .content-frame {
        resize: none;
        color: var(--bluetext, #47536b);
        text-align: left;
        align-self: stretch;
        flex: 1;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 7px 9px 7px 9px;
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 290px;
        height: 66px;
        position: absolute;
        left: 0px;
        top: 31px;
        overflow-y: scroll;
    }

    .prompt-text {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        align-self: stretch;
        flex: 1;
    }

    .prompt2 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 20px;
        top: 20px;
    }

    .tags3 {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 407px;
        height: 906px;
        position: absolute;
        left: 20px;
        top: 64px;
        box-shadow: var(
            --shadow-section-2-box-shadow,
            0px 4px 10px 0px rgba(31, 37, 71, 0.15)
        );
        overflow: hidden;
    }

    .styles {
        width: 350px;
        height: 419px;
        position: absolute;
        left: 20px;
        top: 751px;
    }

    .styles2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .optional {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: 300 16px "IBM Plex Sans", sans-serif;
        position: absolute;
        left: 50px;
        top: 0px;
    }

    .drop-down3 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        width: 100px;
        position: absolute;
        left: 250px;
        top: 3px;
    }

    ._8-px-caret-sort3 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .frame-18 {
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 0px;
        top: 40px;
    }

    .chip {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        flex-shrink: 0;
        position: relative;
    }

    .chip2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .chip3 {
        background: var(--bluemid, #94a0b8);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        flex-shrink: 0;
        position: relative;
    }

    .chip4 {
        color: var(--white, #fafafa);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .divider2 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 350px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 417px;
    }

    .styles-kacheln {
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 350px;
        position: absolute;
        left: 0px;
        top: 78px;
    }

    .kachel {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        padding: 5px;
        display: flex;
        flex-direction: row;
        gap: 2px;
        align-items: center;
        justify-content: flex-start;
        flex-wrap: wrap;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }

    .vorschau {
        border-radius: 2px;
        flex-shrink: 0;
        width: 70px;
        height: 52px;
        position: relative;
        overflow: hidden;
    }

    .rectangle-82 {
        width: 71px;
        height: 52px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._10-px-checkbox {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .photography {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        width: 58px;
        height: 15px;
    }

    ._10-px-checkbox2 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox3 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox4 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .rectangle-7 {
        border-radius: 2px;
        width: 70px;
        height: 52px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._10-px-checkbox5 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox6 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox7 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox8 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .techniques {
        width: 350px;
        height: 417px;
        position: absolute;
        left: 20px;
        top: 314px;
    }

    .techniques2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .optional2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: 300 16px "IBM Plex Sans", sans-serif;
        position: absolute;
        left: 89px;
        top: 0px;
    }

    ._8-px-caret-sort4 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .techniques-tabs {
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 350px;
        position: absolute;
        left: 0px;
        top: 40px;
    }

    .show-more {
        position: absolute;
        inset: 0;
    }

    ._10-px-chevron-down {
        position: absolute;
        left: 0px;
        top: 388px;
        overflow: visible;
    }

    .show-more2 {
        color: var(--blueaccent, #195de5);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 11px;
        top: 385px;
    }

    .divider3 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 350px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 415px;
    }

    .techniques-kacheln {
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 350px;
        position: absolute;
        left: 0px;
        top: 105px;
    }

    ._10-px-checkbox9 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox10 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox11 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .rectangle-72 {
        width: 71px;
        height: 52px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._10-px-checkbox12 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .airbrush {
        width: 71.5px;
        height: 52px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._10-px-checkbox13 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .kachel2 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--blueaccent, #195de5);
        border-width: 1px;
        padding: 5px;
        display: flex;
        flex-direction: row;
        gap: 2px;
        align-items: center;
        justify-content: flex-start;
        flex-wrap: wrap;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }

    .text {
        flex-shrink: 0;
        width: 71px;
        height: 15px;
        position: relative;
    }

    ._10-px-checkbox-checked-filled {
        position: absolute;
        left: 0px;
        top: 2.5px;
        overflow: visible;
    }

    .photography2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 12px;
        top: 0px;
        width: 59px;
        height: 15px;
    }

    .gouache {
        width: 71.5px;
        height: 52px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    ._10-px-checkbox14 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox-checked-filled2 {
        position: absolute;
        left: 0px;
        top: 2.5px;
        overflow: visible;
    }

    ._10-px-checkbox15 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox16 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox17 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    ._10-px-checkbox18 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .medium {
        width: 350px;
        height: 223px;
        position: absolute;
        left: 20px;
        top: 71px;
    }

    .medium2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .medium-frame {
        display: flex;
        flex-direction: row;
        gap: 10px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 350px;
        position: absolute;
        left: 0px;
        top: 31px;
    }

    .kachel3 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-color: var(--blueaccent, #195de5);
        border-width: 1px;
        padding: 5px;
        display: flex;
        flex-direction: column;
        gap: 3px;
        align-items: flex-end;
        justify-content: flex-start;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
        cursor: pointer;
    }

    .kachel3:active,
    .kachel3:focus {
        outline: 2px solid var(--blue-accent, #195de5) !important;
    }

    .photography3 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        width: 71px;
        height: 16px;
    }

    .kachel4 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--blueaccent, #195de5);
        border-width: 1px;
        padding: 5px;
        display: flex;
        flex-direction: column;
        gap: 3px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }

    .photography4 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        width: 70px;
        height: 15px;
    }

    .kachel5 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        padding: 5px;
        display: flex;
        flex-direction: column;
        gap: 3px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }

    .rectangle-73 {
        border-radius: 2px;
        width: 70px;
        height: 52px;
        position: absolute;
        left: 0px;
        top: 0px;
        filter: blur(2px);
    }

    .new-idea {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        width: 70px;
        height: 15px;
    }

    ._32-px-add-filled {
        flex-shrink: 0;
        position: absolute;
        left: 23px;
        top: 15px;
        overflow: visible;
    }

    .medium-devider {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 350px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 221px;
    }

    .fade {
        background: var(
            --fade,
            linear-gradient(
                180deg,
                rgba(250, 250, 250, 0) 0%,
                rgba(250, 250, 250, 1) 89.58333134651184%
            )
        );
        width: 385px;
        height: 90px;
        position: absolute;
        left: 0px;
        top: 816px;
    }

    .scroll-bar2 {
        width: 7px;
        height: 815px;
        position: absolute;
        left: 385px;
        top: 71px;
    }

    .rectangle-83 {
        background: var(--bluebackground, #edeef3);
        border-radius: 100px;
        width: 7px;
        height: 815px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .rectangle-92 {
        background: var(--bluemid, #94a0b8);
        border-radius: 100px;
        width: 7px;
        height: 271.06px;
        position: absolute;
        left: 0px;
        top: 0px;
    }

    .search {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        width: 170px;
        position: absolute;
        left: 200px;
        top: 25px;
    }

    .search2 {
        color: var(--bluemid, #94a0b8);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    ._8-px-search {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }

    .tags4 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 20px;
        top: 20px;
    }

    .top-bar {
        background: var(--white, #fafafa);
        border-style: solid;
        border-color: rgba(148, 160, 184, 0.5);
        border-width: 0px 0px 1px 0px;
        width: 1920px;
        height: 44px;
        position: absolute;
        left: 0px;
        top: 0px;
        overflow: hidden;
    }

    .green-ellipse {
        background: #ed6a5e;
        border-radius: 50%;
        border-style: solid;
        border-color: #d24f42;
        border-width: 0.5px;
        width: 12px;
        height: 12px;
        position: absolute;
        left: 20px;
        top: 16px;
    }

    .yellow-ellipse {
        background: #f5bf4f;
        border-radius: 50%;
        border-style: solid;
        border-color: #d7a13f;
        border-width: 0.5px;
        width: 12px;
        height: 12px;
        position: absolute;
        left: 40px;
        top: 16px;
    }

    .red-ellipse {
        background: #61c553;
        border-radius: 50%;
        border-style: solid;
        border-color: #51a73d;
        border-width: 0.5px;
        width: 12px;
        height: 12px;
        position: absolute;
        left: 60px;
        top: 16px;
    }

    .projects {
        text-align: left;
        font: 400 16px "IBM Plex Sans", sans-serif;
        position: absolute;
        left: 82px;
        top: 0px;
        width: 380px;
        height: 44px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    .projects-span {
        color: var(--bluemid, #94a0b8);
        font: 400 16px "IBM Plex Sans", sans-serif;
    }

    .projects-span2 {
        color: var(--bluetext, #47536b);
        font: 400 16px "IBM Plex Sans", sans-serif;
    }

    a:link {
        text-decoration: none;
    }

    a:visited {
        text-decoration: none;
    }

    a:hover {
        text-decoration: none;
    }

    a:active {
        text-decoration: none;
    }

    :global(.bild:focus) {
        outline: 2px solid var(--blue-accent, #195de5) !important;
    }
</style>
