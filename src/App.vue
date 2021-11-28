<template>
  <div id="app">
    <img
        :src="store.images[0]"
        v-if="store.images[0]"
        class="image"
    />
  </div>
</template>

<script lang="ts">
import { observe } from 'mobx';
import {
    Component,
    Vue,
} from 'vue-property-decorator';
import store from '../github.com/caml-js/store';

@Component({
  components: {

  },
})
export default class App extends Vue {
    store: any = store;
    disposer: any;
    mounted() {
        this.disposer = observe(this.store.title, (delta: any) => {
            document.title = delta.object;
        });
        document.title = store.title;
        const searchParams = new URLSearchParams(window.location.search as string);
        const text = searchParams.get('interval');
        let interval = 5000;
        if(text) {
            interval = parseInt(text);
        }
        setInterval(() => {
            this.store.addReducer((records: any) => {
                return records;
            });
        }, interval);
    }
    destroyed() {
        this.disposer();
    }
}
</script>

<style lang="sass">
html, body
    margin: 0
    width: 100%
    height: 100%

#app
    position: relative
    overflow: hidden
    height: 100%

.image
    width: 100% 
    position: absolute
    top: 50%
    left: 50%
    transform: translate(-50%, -50%)

</style>
