<script>
  //@ts-nocheck
  import axios from 'axios';
    import PrimaryButton from './PrimaryButton.svelte';

  async function fetchData() {
      try {
        const value = document.getElementById("inputBox-1").value;
        const key = ""
        const response = await axios.get('https://youtube.googleapis.com/youtube/v3/search?part=snippet&maxResults=1&q='+ value + '&type=channel&key='+ key);
        if (response.status == 200) {
            const res = response.data;
            let snippet = res["items"][0]["snippet"]

            let aContent = document.getElementById("textField-1") ;
            aContent.textContent = `${snippet["title"]}`;
            aContent.href = `https://www.youtube.com/channel/${res["items"][0]["id"]["channelId"]}`;

            let p1 = document.getElementById("t1") ;
            let des = `${String(snippet["description"])}`
            p1.textContent = `${des.subString(0, 16)}...`

            let imageLink = res["items"][0]["snippet"]["thumbnails"]["default"]["url"];
            let imgStyle = document.getElementById("imgField-1").style;
            imgStyle.backgroundImage = `url("${imageLink}")`;
        } else {
            alert("저런...200이 아니랍니다!");
        }
      } catch (error) {
        console.error('Error fetching data:', error);
      }
  }
</script>

<div class="container">
    <img id="imgField-1" alt="" height="200px" weight='200px'><br />
    <!-- svelte-ignore a11y-missing-content -->
     <strong><a id="textField-1"></a><br /></strong>
     <p id="t1"></p><br />
    <input type="text" id="inputBox-1" placeholder="komq"><br />
   <PrimaryButton on:multiplyclick={fetchData} name="click"/><br />
</div>

<style>
  .container {
    border: 1px solid black;
    border-radius: 20px;
    text-align: center;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 230px;
    height: 300px;
  }
  #imgField-1 {
    margin-top: 35px;
    background-color: white;
    border: 0px solid white;
    height: 88px;
    width: 88px;
  }

  input {
    height: 20px;
    width: 190px;
    margin-bottom: 5px;
  }
  
  a {
    font-size: 1.75em;
    color: black;
    font-style: normal;
  }

  p {
    margin-top: 5px;
  }
</style>


