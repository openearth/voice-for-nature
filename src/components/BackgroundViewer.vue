<template>
  <div>
    <TresCanvas window-size clear-color="#82DBC5" ref="canvas">
      <TresPerspectiveCamera visible ref="camera" :position="currentLocation.camera" :look-at="[150, 0, 100]"/>
      <OrbitControls :target="currentLocation.position" :enablePan="false" :enableZoom="false"/>
      <TresGridHelper />
      <Suspense>
        <TresMesh :position="[20, 0, 20]" :look-at="currentLocation.camera">
          <Text3D :font="fontPath" text="Chat" @click="$router.push('/chat')" :size="1" />
        </TresMesh>
      </Suspense>
      <Suspense>
        <TresMesh :position="[100, 0, 100]" :look-at="currentLocation.camera">
          <Text3D :font="fontPath" text="Measurements" @click="$router.push('/stories/measurements')" :size="1" />
        </TresMesh>
      </Suspense>
      <Suspense>
        <TresMesh :position="[50, 0, 200]" :look-at="currentLocation.camera">
          <Text3D :font="fontPath" text="History" @click="$router.push('/stories/history')" :size="1" />
        </TresMesh>
      </Suspense>
      <Suspense>
        <TresMesh :position="[50, 20, 50]" :look-at="currentLocation.camera">
          <Text3D :font="fontPath" text="Birds" @click="$router.push('/stories/birds')" :size="1" />
        </TresMesh>
      </Suspense>
      <Suspense>
        <Environment
          ref = "env"
          :background="true"
          :files="`../360photos/${currentLocation.id}.hdr`"
        />
      </Suspense>
      <TresScene
        ref="scene">
        <TresDirectionalLight
          :position="[0, 8, 4]"
        />
        <TresMesh
          v-for="location in locations"
          :key="location.id"
          :position="location.position"
          :ref="location.id"
          :visible="location.id !== currentId"
          @click="onClick(location.id)"
          @pointer-enter="onPointerEnter"
          @pointer-leave="onPointerLeave"
          >
          <TresSphereGeometry />
          <TresMeshToonMaterial color="#FBB03B" />
        </TresMesh>
      </TresScene>
    </TresCanvas>
  </div>
</template>

<script>
import { OrbitControls, Environment, Text3D} from '@tresjs/cientos'
import { TresCanvas } from '@tresjs/core'

export default {
  name: 'BackgroundViewer',

  components: {
    TresCanvas,
    OrbitControls,
    Environment,
    Text3D
  },
  data () {
    return {
      fontPath: 'https://raw.githubusercontent.com/Tresjs/assets/main/fonts/FiraCodeRegular.json',
      currentId: "loc1_road",
      locations: [
        {
          id: "loc1_road",
          position: [40, 0, 0],
          camera: [39, 0, -1],
          rotation: 1.6
        }, {
          id: "loc2_pavilion",
          position: [0, 0, 90],
          camera: [-1, 0, 90],
          rotation: 3.0
        }, {
          id: "loc3_tetra",
          position: [30, 0, 180],
          camera: [29, 0, 181],
          rotation: 3.8
        }, {
          id: "loc4_farside",
          position: [110, 0, 250],
          camera: [110, 0, 251],
          rotation: 4.3
        }, {
          id: "loc5_bench",
          position: [160, 0, 190],
          camera: [161, 0, 190],
          rotation: 5.7
        }, {
          id: "loc6_sluice",
          position: [160, 0, 80],
          camera: [161, 0, 80],
          rotation: 0.3
        }, {
          id: "loc7_other",
          position: [120, 0, 40],
          camera: [121, 0, 39],
          rotation: 1.55
        }
      ]
    }
  },
  computed: {
    currentLocation () {
      return this.locations.find(loc => loc.id === this.currentId)
    }
  },
  mounted () {
    console.log(this.$refs)
    this.rotateEnvironment(this.currentLocation.rotation)
  },
  methods: {
    onPointerEnter(ev) {
      if (ev) {
        ev.object.material.color.set('#1CEDDC');
      }
    },
    onPointerLeave(ev) {
      if (ev) {
        ev.object.material.color.set('#efefef')
      }
    },
    onClick(locationId) {
      console.log(locationId)
      this.currentId = locationId
      this.rotateEnvironment(this.currentLocation.rotation)
    },
    rotateEnvironment(rad) {
      this.$refs.scene.parent.backgroundRotation.y = rad
    }
  }
}
</script>

<style>
</style>