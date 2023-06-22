<script>
    let userprompt = "";
    let result = "";

    async function sendPrompt() {
        console.log(userprompt);

        // let response = await fetch("http://127.0.0.1:7860/sdapi/v1/txt2img", {
        //     method: "POST",
        //     headers: {
        //         "Content-Type": "application/json",
        //         Accept: "application/json",
        //     },
        //     body: JSON.stringify(buildPrompt()),
        // });

        // console.log(response);

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
                console.log(data);
                var image = new Image();
                image.src = "data:image/png;base64,"+ data['images'][0];
                result = image.src;
            })
            .catch((err) => console.log(err));
    }

    function buildPrompt() {
        let rawprompt = {
            prompt: "maltese puppy",
            steps: 20,
        };

        return rawprompt;
    }
</script>

<div class="Mainframe">
    <div class="style">
        <div class="tags">
            <div class="tags2">Tags</div>

            <div class="scroll-bar">
                <div class="rectangle-8" />

                <div class="rectangle-9" />
            </div>

            <div class="medium">
                <div class="medium2">Medium</div>

                <div class="kacheln">
                    <div class="kachel">
                        <div class="photography">Photography</div>

                        <img class="rectangle-7" src="rectangle-7.png" />
                    </div>

                    <div class="kachel2">
                        <div class="photography">Graphic</div>

                        <img class="rectangle-7" src="rectangle-7.png" />
                    </div>

                    <div class="kachel3">
                        <div class="photography">Rendering</div>

                        <img class="rectangle-7" src="rectangle-7.png" />
                    </div>

                    <div class="kachel4">
                        <div class="photography">Print</div>

                        <img class="rectangle-7" src="rectangle-7.png" />
                    </div>

                    <div class="kachel5">
                        <div class="photography2">Painting</div>

                        <img class="rectangle-7" src="rectangle-7.png" />
                    </div>

                    <div class="kachel6">
                        <div class="photography">Digital Art</div>

                        <img class="rectangle-7" src="rectangle-7.png" />
                    </div>

                    <div class="kachel7">
                        <div class="photography">Drawing</div>

                        <img class="rectangle-7" src="rectangle-7.png" />
                    </div>
                </div>

                <div class="kachel8">
                    <div class="new-idea">New Idea</div>

                    <div class="frame-1">
                        <img class="rectangle-72" src="rectangle-72.png" />
                    </div>

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

                <div class="divider" />
            </div>

            <div class="techniques">
                <div class="techniques2">Techniques</div>

                <div class="optional">optional</div>

                <div class="drop-down">
                    <div class="drop-down2">most relevant</div>

                    <svg
                        class="_8-px-caret-sort"
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
                        <div class="chip2">Photography</div>
                    </div>

                    <div class="chip3">
                        <div class="chip2">Rendering</div>
                    </div>

                    <div class="chip4">
                        <div class="chip2">Drawing</div>
                    </div>

                    <div class="chip5">
                        <div class="chip2">Graphic</div>
                    </div>

                    <div class="chip6">
                        <div class="chip2">Print</div>
                    </div>

                    <div class="chip7">
                        <div class="chip2">Digital Art</div>
                    </div>

                    <div class="chip8">
                        <div class="chip2">Other</div>
                    </div>

                    <div class="chip9">
                        <div class="chip10">Painting</div>
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

                <div class="divider2" />

                <div class="kacheln2">
                    <div class="kachel">
                        <div class="photography3">Oil</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel2">
                        <div class="photography3">Chiaroscuro</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox2"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel9">
                        <div class="photography3">Pointilism</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox3"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel3">
                        <div class="photography3">Ink</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox4"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel10">
                        <div class="photography4">Watercolor</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox-checked-filled"
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
                    </div>

                    <div class="kachel11">
                        <div class="photography3">Impasto</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox5"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel12">
                        <div class="photography3">Brush</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox6"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel6">
                        <div class="photography3">Sgraffito</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox7"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel13">
                        <div class="photography3">Stippling</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox8"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel7">
                        <div class="photography3">Graffiti</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox9"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel14">
                        <div class="photography4">Alla Prima</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox-checked-filled2"
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
                    </div>

                    <div class="kachel15">
                        <div class="photography3">Glazing</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox10"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>
                </div>
            </div>

            <div class="styles">
                <div class="styles2">Styles</div>

                <div class="optional2">optional</div>

                <div class="drop-down">
                    <div class="drop-down2">most relevant</div>

                    <svg
                        class="_8-px-caret-sort2"
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

                <div class="style-tabs">
                    <div class="chip11">
                        <div class="chip2">Effects</div>
                    </div>

                    <div class="chip11">
                        <div class="chip2">Materials</div>
                    </div>

                    <div class="chip11">
                        <div class="chip2">Concepts</div>
                    </div>

                    <div class="chip12">
                        <div class="chip10">Movements</div>
                    </div>

                    <div class="chip11">
                        <div class="chip2">Other</div>
                    </div>
                </div>

                <div class="divider3" />

                <div class="kacheln3">
                    <div class="kachel">
                        <div class="photography3">Cubism</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox11"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel2">
                        <div class="photography3">Descriptions</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox12"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel3">
                        <div class="photography3">Surrealism</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox13"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel4">
                        <div class="photography3">Descriptions</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox14"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel12">
                        <div class="photography3">Renaissance</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox15"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel6">
                        <div class="photography3">Descriptions</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox16"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel7">
                        <div class="photography3">Pop Art</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox17"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>

                    <div class="kachel16">
                        <div class="photography3">Descriptions</div>

                        <img class="rectangle-7" src="rectangle-7.png" />

                        <svg
                            class="_8-px-checkbox18"
                            width="10"
                            height="10"
                            viewBox="0 0 10 10"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                d="M8.125 1.25H1.875C1.70924 1.25 1.55027 1.31585 1.43306 1.43306C1.31585 1.55027 1.25 1.70924 1.25 1.875V8.125C1.25 8.29076 1.31585 8.44973 1.43306 8.56694C1.55027 8.68415 1.70924 8.75 1.875 8.75H8.125C8.29076 8.75 8.44973 8.68415 8.56694 8.56694C8.68415 8.44973 8.75 8.29076 8.75 8.125V1.875C8.75 1.70924 8.68415 1.55027 8.56694 1.43306C8.44973 1.31585 8.29076 1.25 8.125 1.25V1.25ZM1.875 8.125V1.875H8.125V8.125H1.875Z"
                                fill="#195DE6"
                            />
                        </svg>
                    </div>
                </div>
            </div>

            <div class="rectangle-23" />
        </div>

        <div class="prompt">
            <div class="prompt2">Prompt</div>

            <div class="negative-content">
                <div class="negative-content2">Negative Content</div>

                <div class="rectangle-25" />

                <div class="what-shouldn-t-be-on-the-image">
                    What shouldn’t be on the image?
                </div>
            </div>

            <div class="content">
                <div class="content2">Content</div>

                <div class="rectangle-24" />
                <input value={userprompt} class="rectangle-24" />

                <!-- <div class="a-mouse-chasing-a-cat">A mouse chasing a cat</div> -->
            </div>

            <div class="raw-prompt">
                <div class="raw-prompt2">Raw Prompt</div>

                <div class="rectangle-24" />

                <div
                    class="a-painting-of-a-mouse-chasing-a-cat-painted-with-watercolors-0-3-in-alla-prima-1-2-style"
                >
                    {userprompt}
                </div>

                <svg
                    class="_16-px-chevron-down"
                    width="16"
                    height="16"
                    viewBox="0 0 16 16"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M8 5.0002L13 10.0002L12.3 10.7002L8 6.40019L3.7 10.7002L3 10.0002L8 5.0002Z"
                        fill="#47536B"
                    />
                </svg>
            </div>

            <div class="tags3">
                <div class="tags4">Tags</div>

                <div class="frame-19">
                    <div class="tag">
                        <div class="frame-192">
                            <svg
                                class="_8-px-close-filled"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 0.5C2.05 0.5 0.5 2.05 0.5 4C0.5 5.95 2.05 7.5 4 7.5C5.95 7.5 7.5 5.95 7.5 4C7.5 2.05 5.95 0.5 4 0.5ZM5.35 5.75L4 4.4L2.65 5.75L2.25 5.35L3.6 4L2.25 2.65L2.65 2.25L4 3.6L5.35 2.25L5.75 2.65L4.4 4L5.75 5.35L5.35 5.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>

                            <div class="tag2">Painting</div>
                        </div>

                        <div class="frame-20">
                            <svg
                                class="_8-px-subtract"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 1C5.65 1 7 2.35 7 4C7 5.65 5.65 7 4 7C2.35 7 1 5.65 1 4C1 2.35 2.35 1 4 1ZM4 0.5C2.075 0.5 0.5 2.075 0.5 4C0.5 5.925 2.075 7.5 4 7.5C5.925 7.5 7.5 5.925 7.5 4C7.5 2.075 5.925 0.5 4 0.5Z"
                                    fill="#FAFAFA"
                                />
                                <path
                                    d="M2 3.75H6V4.25H2V3.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>

                            <div class="_1-0">1,0</div>

                            <svg
                                class="_8-px-add"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 1C5.65 1 7 2.35 7 4C7 5.65 5.65 7 4 7C2.35 7 1 5.65 1 4C1 2.35 2.35 1 4 1ZM4 0.5C2.075 0.5 0.5 2.075 0.5 4C0.5 5.925 2.075 7.5 4 7.5C5.925 7.5 7.5 5.925 7.5 4C7.5 2.075 5.925 0.5 4 0.5Z"
                                    fill="#FAFAFA"
                                />
                                <path
                                    d="M6 3.75H4.25V2H3.75V3.75H2V4.25H3.75V6H4.25V4.25H6V3.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>
                        </div>
                    </div>

                    <div class="tag3">
                        <div class="frame-192">
                            <svg
                                class="_8-px-close-filled2"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 0.5C2.05 0.5 0.5 2.05 0.5 4C0.5 5.95 2.05 7.5 4 7.5C5.95 7.5 7.5 5.95 7.5 4C7.5 2.05 5.95 0.5 4 0.5ZM5.35 5.75L4 4.4L2.65 5.75L2.25 5.35L3.6 4L2.25 2.65L2.65 2.25L4 3.6L5.35 2.25L5.75 2.65L4.4 4L5.75 5.35L5.35 5.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>

                            <div class="tag2">Watercolor</div>
                        </div>

                        <div class="frame-20">
                            <svg
                                class="_8-px-subtract2"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 1C5.65 1 7 2.35 7 4C7 5.65 5.65 7 4 7C2.35 7 1 5.65 1 4C1 2.35 2.35 1 4 1ZM4 0.5C2.075 0.5 0.5 2.075 0.5 4C0.5 5.925 2.075 7.5 4 7.5C5.925 7.5 7.5 5.925 7.5 4C7.5 2.075 5.925 0.5 4 0.5Z"
                                    fill="#FAFAFA"
                                />
                                <path
                                    d="M2 3.75H6V4.25H2V3.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>

                            <div class="_1-0">0,3</div>

                            <svg
                                class="_8-px-add2"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 1C5.65 1 7 2.35 7 4C7 5.65 5.65 7 4 7C2.35 7 1 5.65 1 4C1 2.35 2.35 1 4 1ZM4 0.5C2.075 0.5 0.5 2.075 0.5 4C0.5 5.925 2.075 7.5 4 7.5C5.925 7.5 7.5 5.925 7.5 4C7.5 2.075 5.925 0.5 4 0.5Z"
                                    fill="#FAFAFA"
                                />
                                <path
                                    d="M6 3.75H4.25V2H3.75V3.75H2V4.25H3.75V6H4.25V4.25H6V3.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>
                        </div>
                    </div>

                    <div class="tag4">
                        <div class="frame-192">
                            <svg
                                class="_8-px-close-filled3"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 0.5C2.05 0.5 0.5 2.05 0.5 4C0.5 5.95 2.05 7.5 4 7.5C5.95 7.5 7.5 5.95 7.5 4C7.5 2.05 5.95 0.5 4 0.5ZM5.35 5.75L4 4.4L2.65 5.75L2.25 5.35L3.6 4L2.25 2.65L2.65 2.25L4 3.6L5.35 2.25L5.75 2.65L4.4 4L5.75 5.35L5.35 5.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>

                            <div class="tag2">Alla Prima</div>
                        </div>

                        <div class="frame-20">
                            <svg
                                class="_8-px-subtract3"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 1C5.65 1 7 2.35 7 4C7 5.65 5.65 7 4 7C2.35 7 1 5.65 1 4C1 2.35 2.35 1 4 1ZM4 0.5C2.075 0.5 0.5 2.075 0.5 4C0.5 5.925 2.075 7.5 4 7.5C5.925 7.5 7.5 5.925 7.5 4C7.5 2.075 5.925 0.5 4 0.5Z"
                                    fill="#FAFAFA"
                                />
                                <path
                                    d="M2 3.75H6V4.25H2V3.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>

                            <div class="_1-0">1,2</div>

                            <svg
                                class="_8-px-add3"
                                width="8"
                                height="8"
                                viewBox="0 0 8 8"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M4 1C5.65 1 7 2.35 7 4C7 5.65 5.65 7 4 7C2.35 7 1 5.65 1 4C1 2.35 2.35 1 4 1ZM4 0.5C2.075 0.5 0.5 2.075 0.5 4C0.5 5.925 2.075 7.5 4 7.5C5.925 7.5 7.5 5.925 7.5 4C7.5 2.075 5.925 0.5 4 0.5Z"
                                    fill="#FAFAFA"
                                />
                                <path
                                    d="M6 3.75H4.25V2H3.75V3.75H2V4.25H3.75V6H4.25V4.25H6V3.75Z"
                                    fill="#FAFAFA"
                                />
                            </svg>
                        </div>
                    </div>
                </div>
            </div>

            <div class="divider4" />
        </div>

        <div class="settings">
            <div class="settings2">Settings</div>

            <div class="resolution">
                <div class="input-field">
                    <div class="input">512px</div>
                </div>

                <div class="input-field2">
                    <div class="input">512px</div>
                </div>

                <div class="width">Width</div>

                <div class="hight">Hight</div>

                <div class="resolution2">Resolution</div>
            </div>

            <div class="divider5" />

            <div class="model">
                <div class="drop-down3">
                    <div class="drop-down2">v1-5-pruned-ckpt</div>

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
                <div class="drop-down3">
                    <div class="drop-down2">Euler a</div>

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

            <div class="guidance">
                <div class="guidance-scale">Guidance Scale</div>

                <div class="input-field4">
                    <div class="input">15</div>
                </div>

                <div class="slider">
                    <div class="rectangle-252" />

                    <div class="ellipse-12" />
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
                    <div class="input">45</div>
                </div>

                <div class="slider">
                    <div class="rectangle-252" />

                    <div class="ellipse-12" />
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
                <div class="input-field">
                    <div class="input">4</div>
                </div>

                <div class="input-field2">
                    <div class="input">1</div>
                </div>

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

            <div class="divider6" />
        </div>

        <div class="buttons">
            <button class="button" on:click={sendPrompt}> Generate </button>

            <div class="button3">
                <div class="button4">Save Configuration</div>
            </div>

            <div class="button5">
                <div class="button4">Open Configurations</div>
            </div>
        </div>
    </div>

    <div class="top-bar">
        <div class="ellipse-13" />

        <div class="ellipse-15" />

        <div class="ellipse-16" />
    </div>

    <div class="frame-32">
        <div class="history">
            <div class="frame-25">
                <img class="_00009-1938723002" src="_00009-1938723002.png" />

                <img class="_00014-446255131" src="_00014-446255131.png" />

                <img class="_00012-3684755115" src="_00012-3684755115.png" />

                <img
                    class="_00013-3684755116-6"
                    src="_00013-3684755116-6.png"
                />
            </div>

            <div class="frame-252">
                <div class="frame-31">
                    <img
                        class="_00005-3807344773"
                        src="_00005-3807344773.png"
                    />

                    <img
                        class="_00004-3807344772"
                        src="_00004-3807344772.png"
                    />

                    <img class="_00015-446255132" src="_00015-446255132.png" />

                    <img class="_00016-446255133" src="_00016-446255133.png" />
                </div>

                <div class="frame-30">
                    <img
                        class="_00007-1938723000"
                        src="_00007-1938723000.png"
                    />

                    <img
                        class="_00003-3807344771"
                        src="_00003-3807344771.png"
                    />
                </div>
            </div>

            <div class="frame-26">
                <img class="_00011-3684755114" src="_00011-3684755114.png" />

                <img class="_00002-3807344770" src="_00002-3807344770.png" />

                <img class="_00006-1938722999" src="_00006-1938722999.png" />

                <img class="_00008-1938723001" src="_00008-1938723001.png" />
            </div>

            <div class="frame-28">
                <img class="_00011-3684755114" src="_00011-3684755114.png" />

                <img class="_00002-3807344770" src="_00002-3807344770.png" />

                <img class="_00006-1938722999" src="_00006-1938722999.png" />

                <img class="_00008-1938723001" src="_00008-1938723001.png" />
            </div>

            <div class="frame-29">
                <img class="_00011-3684755114" src="_00011-3684755114.png" />

                <img class="_00002-3807344770" src="_00002-3807344770.png" />

                <img class="_00006-1938722999" src="_00006-1938722999.png" />

                <img class="_00008-1938723001" src="_00008-1938723001.png" />
            </div>

            <div class="frame-302">
                <img class="_00011-3684755114" src="_00011-3684755114.png" />

                <img class="_00002-3807344770" src="_00002-3807344770.png" />

                <img class="_00006-1938722999" src="_00006-1938722999.png" />

                <img class="_00008-1938723001" src="_00008-1938723001.png" />
            </div>

            <div class="frame-27">
                <img class="_00001-1445990440" src="_00001-1445990440.png" />

                <img class="_00010-3684755113" src="_00010-3684755113.png" />

                <img class="_00017-446255134" src="_00017-446255134.png" />
            </div>

            <div class="frame-312">
                <img class="_00011-3684755114" src="_00011-3684755114.png" />

                <img class="_00002-3807344770" src="_00002-3807344770.png" />

                <img class="_00006-1938722999" src="_00006-1938722999.png" />

                <img class="_00008-1938723001" src="_00008-1938723001.png" />
            </div>

            <div class="frame-322">
                <img class="_00011-3684755114" src="_00011-3684755114.png" />

                <img class="_00002-3807344770" src="_00002-3807344770.png" />

                <img class="_00006-1938722999" src="_00006-1938722999.png" />

                <img class="_00008-1938723001" src="_00008-1938723001.png" />
            </div>

            <div class="fade" />

            <div class="scroll-bar2">
                <div class="rectangle-82" />

                <div class="rectangle-92" />
            </div>
        </div>

        <div class="gro-e-ansicht">
            <img src={result} alt="Prompt" />

            <!-- <img class="result" src="result.png" /> -->

            <div class="progress-bar">
                <div class="image-4-100">Image 4: 100%</div>
            </div>

            <div class="buttons2">
                <div class="button6">
                    <div class="button7">Save Image</div>
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
        </div>
    </div>
</div>

<style>
    Copy .Mainframe,
    .Mainframe * {
        box-sizing: border-box;
    }
    .Mainframe {
        background: var(--bluebackground, #edeef3);
        width: 1920px;
        height: 1080px;
        position: relative;
        overflow: hidden;
    }
    .style {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 757px;
        height: 1005px;
        position: absolute;
        left: 22px;
        top: 44px;
        box-shadow: var(
            --shadow-section-box-shadow,
            0px 7px 14px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .tags {
        width: 392px;
        height: 1150px;
        position: absolute;
        left: 0px;
        top: 20px;
    }
    .tags2 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 20px;
        top: 0px;
    }
    .scroll-bar {
        width: 7px;
        height: 898px;
        position: absolute;
        left: 385px;
        top: 51px;
    }
    .rectangle-8 {
        background: var(--bluebackground, #edeef3);
        border-radius: 100px;
        width: 7px;
        height: 898px;
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .rectangle-9 {
        background: var(--bluemid, #94a0b8);
        border-radius: 100px;
        width: 7px;
        height: 298.67px;
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .medium {
        width: 350px;
        height: 223px;
        position: absolute;
        left: 20px;
        top: 51px;
    }
    .medium2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .kacheln {
        width: 350px;
        height: 170px;
        position: absolute;
        left: 0px;
        top: 31px;
    }
    .kachel {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 0px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .photography {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 4px;
        top: 60px;
        width: 71px;
        height: 16px;
    }
    .rectangle-7 {
        border-radius: 2px;
        width: 70px;
        height: 52px;
        position: absolute;
        left: 5px;
        top: 5px;
    }
    .kachel2 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 0px;
        top: 90px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .kachel3 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 90px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .kachel4 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 90px;
        top: 90px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .kachel5 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--blueaccent, #195de5);
        border-width: 1px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 180px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .photography2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 5px;
        top: 60px;
        width: 70px;
        height: 15px;
    }
    .kachel6 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 180px;
        top: 90px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .kachel7 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 270px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .kachel8 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 270px;
        top: 121px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .new-idea {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 5px;
        top: 60px;
        width: 70px;
        height: 15px;
    }
    .frame-1 {
        border-radius: 2px;
        width: 70px;
        height: 52px;
        position: absolute;
        left: 5px;
        top: 5px;
        overflow: hidden;
    }
    .rectangle-72 {
        border-radius: 2px;
        width: 70px;
        height: 52px;
        position: absolute;
        left: 0px;
        top: 0px;
        filter: blur(2px);
    }
    ._32-px-add-filled {
        position: absolute;
        left: 23px;
        top: 15px;
        overflow: visible;
    }
    .divider {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 350px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 221px;
    }
    .techniques {
        width: 350px;
        height: 417px;
        position: absolute;
        left: 20px;
        top: 294px;
    }
    .techniques2 {
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
        left: 89px;
        top: 0px;
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
        width: 100px;
        position: absolute;
        left: 250px;
        top: 3px;
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
    .frame-18 {
        width: 339px;
        height: 50px;
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
        position: absolute;
        left: 0px;
        top: 0px;
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
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 85px;
        top: 0px;
    }
    .chip4 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 221px;
        top: 0px;
    }
    .chip5 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 284px;
        top: 0px;
    }
    .chip6 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 0px;
        top: 28px;
    }
    .chip7 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 46px;
        top: 28px;
    }
    .chip8 {
        background: var(--bluebackground, #edeef3);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 118px;
        top: 28px;
    }
    .chip9 {
        background: var(--bluemid, #94a0b8);
        border-radius: 3px;
        padding: 4px 8px 4px 8px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 158px;
        top: 0px;
    }
    .chip10 {
        color: var(--white, #fafafa);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }
    .show-more {
        display: flex;
        flex-direction: row;
        gap: 1px;
        align-items: center;
        justify-content: flex-start;
        width: 66px;
        height: 14px;
        position: absolute;
        left: 0px;
        top: 385px;
    }
    ._10-px-chevron-down {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }
    .show-more2 {
        color: var(--blueaccent, #195de5);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
    }
    .divider2 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 350px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 415px;
    }
    .kacheln2 {
        width: 350px;
        height: 260px;
        position: absolute;
        left: 0px;
        top: 105px;
    }
    .photography3 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 17px;
        top: 60px;
        width: 58px;
        height: 15px;
    }
    ._8-px-checkbox {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox2 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .kachel9 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 0px;
        top: 180px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    ._8-px-checkbox3 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox4 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .kachel10 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--blueaccent, #195de5);
        border-width: 1px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 90px;
        top: 90px;
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
        position: absolute;
        left: 16px;
        top: 60px;
        width: 59px;
        height: 15px;
    }
    ._8-px-checkbox-checked-filled {
        position: absolute;
        left: 4px;
        top: 62px;
        overflow: visible;
    }
    .kachel11 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 90px;
        top: 180px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    ._8-px-checkbox5 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .kachel12 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 180px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    ._8-px-checkbox6 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox7 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .kachel13 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 180px;
        top: 180px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    ._8-px-checkbox8 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox9 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .kachel14 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--blueaccent, #195de5);
        border-width: 1px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 270px;
        top: 90px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    ._8-px-checkbox-checked-filled2 {
        position: absolute;
        left: 4px;
        top: 62px;
        overflow: visible;
    }
    .kachel15 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 270px;
        top: 180px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    ._8-px-checkbox10 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .styles {
        width: 350px;
        height: 419px;
        position: absolute;
        left: 20px;
        top: 731px;
    }
    .styles2 {
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
        left: 50px;
        top: 0px;
    }
    ._8-px-caret-sort2 {
        flex-shrink: 0;
        position: relative;
        overflow: visible;
    }
    .style-tabs {
        display: flex;
        flex-direction: row;
        align-items: flex-start;
        justify-content: space-between;
        position: absolute;
        left: 0px;
        top: 40px;
    }
    .chip11 {
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
    .chip12 {
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
    .divider3 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 350px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 417px;
    }
    .kacheln3 {
        width: 350px;
        height: 170px;
        position: absolute;
        left: 0px;
        top: 78px;
    }
    ._8-px-checkbox11 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox12 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox13 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox14 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox15 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox16 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    ._8-px-checkbox17 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .kachel16 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        width: 80px;
        height: 80px;
        position: absolute;
        left: 270px;
        top: 90px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    ._8-px-checkbox18 {
        position: absolute;
        left: 5px;
        top: 62px;
        overflow: visible;
    }
    .rectangle-23 {
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
        top: 894px;
    }
    .prompt {
        width: 290px;
        height: 491px;
        position: absolute;
        left: 448px;
        top: 20px;
    }
    .prompt2 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .negative-content {
        width: 290px;
        height: 97px;
        position: absolute;
        left: 0px;
        top: 158px;
    }
    .negative-content2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .rectangle-25 {
        background: var(--bluebackground, #edeef3);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        width: 290px;
        height: 66px;
        position: absolute;
        left: 0px;
        top: 31px;
    }
    .what-shouldn-t-be-on-the-image {
        color: var(--bluemid, #94a0b8);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 8px;
        top: 39px;
        width: 274px;
        height: 34px;
    }
    .content {
        width: 290px;
        height: 97px;
        position: absolute;
        left: 0px;
        top: 51px;
    }
    .content2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .rectangle-24 {
        background: var(--bluebackground, #edeef3);
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        border-radius: 6px;
        border-style: solid;
        border-color: var(--bluemid, #94a0b8);
        border-width: 1px;
        width: 290px;
        height: 66px;
        position: absolute;
        left: 0px;
        top: 31px;
    }
    .a-mouse-chasing-a-cat {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 8px;
        top: 39px;
        width: 274px;
        height: 34px;
    }
    .raw-prompt {
        width: 290px;
        height: 97px;
        position: absolute;
        left: 0px;
        top: 372px;
    }
    .raw-prompt2 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .a-painting-of-a-mouse-chasing-a-cat-painted-with-watercolors-0-3-in-alla-prima-1-2-style {
        text-align: left;
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
        position: absolute;
        left: 8px;
        top: 39px;
        width: 274px;
        height: 34px;
    }
    .a-painting-of-a-mouse-chasing-a-cat-painted-with-watercolors-0-3-in-alla-prima-1-2-style-span {
        color: var(--blueaccent, #195de5);
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
    }
    .a-painting-of-a-mouse-chasing-a-cat-painted-with-watercolors-0-3-in-alla-prima-1-2-style-span2 {
        color: var(--bluetext, #47536b);
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
    }
    .a-painting-of-a-mouse-chasing-a-cat-painted-with-watercolors-0-3-in-alla-prima-1-2-style-span3 {
        color: var(--blueaccent, #195de5);
        font: var(--text-mono, 400 11px "IBM Plex Mono", sans-serif);
    }
    ._16-px-chevron-down {
        position: absolute;
        left: 290px;
        top: 20px;
        transform: translate(-16px, -16px);
        overflow: visible;
    }
    .tags3 {
        display: flex;
        flex-direction: column;
        gap: 10px;
        align-items: flex-start;
        justify-content: flex-start;
        width: 290px;
        position: absolute;
        left: 0px;
        top: 265px;
    }
    .tags4 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--h-2, 500 16px "IBM Plex Sans", sans-serif);
        position: relative;
        align-self: stretch;
    }
    .frame-19 {
        background: #edeef3;
        border-radius: 6px;
        align-self: stretch;
        flex-shrink: 0;
        height: 66px;
        position: relative;
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
        position: absolute;
        left: 8px;
        top: 8px;
    }
    .frame-192 {
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
    .tag3 {
        background: var(--blueaccent, #195de5);
        border-radius: 3px;
        padding: 4px 6px 4px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 122px;
        top: 8px;
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
    .tag4 {
        background: var(--blueaccent, #195de5);
        border-radius: 3px;
        padding: 4px 6px 4px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: center;
        justify-content: flex-start;
        position: absolute;
        left: 8px;
        top: 36px;
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
    .divider4 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 290px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 489px;
    }
    .settings {
        width: 290px;
        height: 364px;
        position: absolute;
        left: 448px;
        top: 531px;
    }
    .settings2 {
        color: var(--blue-heading, #1f4ead);
        text-align: left;
        font: var(--h-1, 500 24px "IBM Plex Sans", sans-serif);
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .resolution {
        width: 126px;
        height: 72px;
        position: absolute;
        left: 0px;
        top: 270px;
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
    .divider5 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 290px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 248px;
    }
    .model {
        width: 135px;
        height: 44px;
        position: absolute;
        left: 0px;
        top: 56px;
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
        width: 135px;
        position: absolute;
        left: 0px;
        top: 22px;
    }
    ._8-px-caret-sort3 {
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
        left: 155px;
        top: 56px;
    }
    ._8-px-caret-sort4 {
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
        left: 0px;
        top: 184px;
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
    .guidance {
        width: 135px;
        height: 44px;
        position: absolute;
        left: 0px;
        top: 120px;
    }
    .guidance-scale {
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
    .rectangle-252 {
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
        left: 155px;
        top: 120px;
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
        left: 155px;
        top: 270px;
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
    .divider6 {
        background: var(--bluebackground, #edeef3);
        border-radius: 2px;
        width: 290px;
        height: 2px;
        position: absolute;
        left: 0px;
        top: 362px;
    }
    .buttons {
        width: 290px;
        height: 70px;
        position: absolute;
        left: 448px;
        top: 915px;
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
        width: 290px;
        position: absolute;
        left: 0px;
        top: 40px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .button2 {
        color: var(--white, #fafafa);
        text-align: center;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: relative;
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .button3 {
        background: var(--blueaccent-light, #d1dffa);
        border-radius: 4px;
        padding: 8px 12px 8px 12px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: center;
        width: 140px;
        position: absolute;
        left: 0px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .button4 {
        color: var(--blueaccent, #195de5);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }
    .button5 {
        background: var(--blueaccent-light, #d1dffa);
        border-radius: 4px;
        padding: 8px 12px 8px 12px;
        display: flex;
        flex-direction: row;
        gap: 0px;
        align-items: flex-start;
        justify-content: center;
        width: 140px;
        position: absolute;
        left: 150px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .top-bar {
        background: var(--white, #fafafa);
        width: 1920px;
        height: 24px;
        position: absolute;
        left: 0px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .ellipse-13 {
        background: #ed6a5e;
        border-radius: 50%;
        border-style: solid;
        border-color: #d24f42;
        border-width: 0.5px;
        width: 12px;
        height: 12px;
        position: absolute;
        left: 10px;
        top: 6px;
    }
    .ellipse-15 {
        background: #f5bf4f;
        border-radius: 50%;
        border-style: solid;
        border-color: #d7a13f;
        border-width: 0.5px;
        width: 12px;
        height: 12px;
        position: absolute;
        left: 30px;
        top: 6px;
    }
    .ellipse-16 {
        background: #61c553;
        border-radius: 50%;
        border-style: solid;
        border-color: #51a73d;
        border-width: 0.5px;
        width: 12px;
        height: 12px;
        position: absolute;
        left: 50px;
        top: 6px;
    }
    .frame-32 {
        background: var(--white, #fafafa);
        border-radius: 12px;
        width: 1073px;
        height: 1005px;
        position: absolute;
        left: 827px;
        top: 44px;
        box-shadow: var(
            --shadow-section-box-shadow,
            0px 7px 14px 0px rgba(31, 37, 71, 0.25)
        );
        overflow: hidden;
    }
    .history {
        width: 407px;
        height: 1034px;
        position: absolute;
        left: 666px;
        top: -70px;
    }
    .frame-25 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 942px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
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
    .frame-252 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: column;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 535px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .frame-31 {
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-shrink: 0;
        position: relative;
    }
    ._00005-3807344773 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00004-3807344772 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00015-446255132 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00016-446255133 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    .frame-30 {
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        flex-shrink: 0;
        position: relative;
    }
    ._00007-1938723000 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00003-3807344771 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    .frame-26 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 835px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    ._00011-3684755114 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00002-3807344770 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00006-1938722999 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00008-1938723001 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    .frame-28 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 428px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .frame-29 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 321px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .frame-302 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 214px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .frame-27 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 728px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    ._00001-1445990440 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00010-3684755113 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    ._00017-446255134 {
        border-radius: 4px;
        flex-shrink: 0;
        width: 80px;
        height: 80px;
        position: relative;
    }
    .frame-312 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 107px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
    }
    .frame-322 {
        background: var(--bluebackground, #edeef3);
        border-radius: 10px;
        padding: 6px 6px 6px 6px;
        display: flex;
        flex-direction: row;
        gap: 6px;
        align-items: flex-start;
        justify-content: flex-start;
        position: absolute;
        left: 20px;
        top: 0px;
        box-shadow: var(
            --shadow-kachel-box-shadow,
            0px 2px 3px 0px rgba(31, 37, 71, 0.25)
        );
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
        width: 407px;
        height: 68px;
        position: absolute;
        left: 407px;
        top: 138px;
        transform-origin: 0 0;
        transform: rotate(-180deg) scale(1, 1);
    }
    .scroll-bar2 {
        width: 7px;
        height: 949px;
        position: absolute;
        left: 392px;
        top: 1034px;
        transform-origin: 0 0;
        transform: rotate(-180deg) scale(1, 1);
    }
    .rectangle-82 {
        background: var(--bluebackground, #edeef3);
        border-radius: 100px;
        width: 7px;
        height: 949px;
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .rectangle-92 {
        background: var(--bluemid, #94a0b8);
        border-radius: 100px;
        width: 7px;
        height: 315.63px;
        position: absolute;
        left: 0px;
        top: 0px;
    }
    .gro-e-ansicht {
        display: flex;
        flex-direction: column;
        gap: 19px;
        align-items: center;
        justify-content: flex-start;
        width: 606px;
        height: 697px;
        position: absolute;
        left: 20px;
        top: 20px;
    }
    .result {
        background: linear-gradient(to left, #fafafa, #fafafa);
        flex-shrink: 0;
        width: 606px;
        height: 606px;
        position: relative;
        box-shadow: 0px 2px 3px 0px rgba(31, 37, 71, 0.25);
        overflow: hidden;
    }
    .progress-bar {
        background: linear-gradient(to left, #d1dffa, #d1dffa),
            linear-gradient(to left, #edeef3, #edeef3);
        padding: 0px 8px 0px 8px;
        display: flex;
        flex-direction: column;
        gap: 0px;
        align-items: flex-start;
        justify-content: center;
        flex-shrink: 0;
        width: 606px;
        height: 22px;
        position: relative;
        overflow: hidden;
    }
    .image-4-100 {
        color: var(--bluetext, #47536b);
        text-align: left;
        font: var(--text, 400 11px "IBM Plex Sans", sans-serif);
        position: relative;
        width: 114px;
        height: 14px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }
    .buttons2 {
        flex-shrink: 0;
        width: 606px;
        height: 30px;
        position: relative;
    }
    .button6 {
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
    .button7 {
        color: var(--white, #fafafa);
        text-align: left;
        font: var(--h-3, 500 11px "IBM Plex Sans", sans-serif);
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-start;
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
</style>
