<template>
  <div class="flex items-baseline-last justify-between gap-4">
    <div>
      <div class="text-4xl" id="clock">{{ currentTime }}</div>
      <div class="text-4xl" id="date">{{ currentDate }}</div>
    </div>

    <div class="hidden">
      <label for="timezone">Select Timezone:</label>
      <select id="timezone" ref="timezoneRef" @change="updateClock">
        <option value="UTC">UTC</option>
        <option value="America/New_York">USA (New York)</option>
        <option value="America/Los_Angeles">USA (Los Angeles)</option>
        <option value="Europe/London">UK (London)</option>
        <option value="Europe/Berlin">Germany (Berlin)</option>
        <option value="Asia/Kolkata" selected>India (Kolkata)</option>
        <!-- ✅ Default -->
        <option value="Asia/Tokyo">Japan (Tokyo)</option>
        <option value="Australia/Sydney">Australia (Sydney)</option>
      </select>

      <label for="format">Select Time Format:</label>
      <select id="format" ref="formatRef" @change="updateClock">
        <option value="24" selected>24-Hour</option>
        <!-- ✅ Default -->
        <option value="12">12-Hour (AM/PM)</option>
      </select>
    </div>

    <Dialog>
      <DialogTrigger as-child>
        <Button variant="outline"> <EllipsisVertical /> </Button>
      </DialogTrigger>
      <DialogContent class="sm:max-w-[425px]">
        <DialogHeader>
          <DialogTitle>Clock Time & Location</DialogTitle>
          <DialogDescription>
            Make changes to your Clock here. Select Time Format and Location.
          </DialogDescription>
        </DialogHeader>
        <div class="grid gap-4 py-4">
          <div class="grid grid-cols-4 items-center gap-4">
            <Label for="name" class="text-right"> Name </Label>
            <Input id="name" value="Pedro Duarte" class="col-span-3" />
          </div>
          <div class="grid grid-cols-4 items-center gap-4">
            <Label for="username" class="text-right"> Username </Label>
            <Input id="username" value="@peduarte" class="col-span-3" />
          </div>
        </div>
        <DialogFooter>
          <Button type="submit"> Save changes </Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { EllipsisVertical } from 'lucide-vue-next'
import { Button } from '@/components/ui/button'
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog'

const currentTime = ref('--:--:--')
const currentDate = ref('Loading...')
const timezoneRef = ref(null)
const formatRef = ref(null)

function updateClock() {
  const timeZone = timezoneRef.value?.value || 'Asia/Kolkata'
  const use12Hour = formatRef.value?.value === '12'
  const now = new Date()

  const timeFormatter = new Intl.DateTimeFormat('en-US', {
    hour: '2-digit',
    minute: '2-digit',
    // second: '2-digit',
    hour12: use12Hour,
    timeZone,
  })

  const dateFormatter = new Intl.DateTimeFormat('en-US', {
    weekday: 'long',
    // year: 'numeric',
    month: 'short',
    day: 'numeric',
    timeZone,
  })

  currentTime.value = timeFormatter.format(now)
  currentDate.value = dateFormatter.format(now)
}

onMounted(() => {
  updateClock()
  setInterval(updateClock, 1000)
})
</script>
