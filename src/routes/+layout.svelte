<script>
  import "./styles.css";
  import Header from "../components/Header.svelte";
  import Footer from "../components/Footer.svelte";
  import Whatsapp from "../components/Whatsapp.svelte";
</script>

<Header />
<slot />
<Footer />
<Whatsapp/>
