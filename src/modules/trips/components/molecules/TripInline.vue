<script lang="ts" setup>
import { type Trip, TripType } from "@/interfaces/trip.interface";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import Button from "@/components/molecules/Button.vue";
import { DateTime } from "luxon";
import { computed } from "vue";
import defaultProfileImage from "@/assets/images/logos/logo_white.svg";

const props = defineProps<{
  trip: Trip;
}>();

const bgTypeColor = computed((): string => {
  switch (props.trip.type) {
    case TripType.DRIVER:
      return "bg-main-base";
    default:
      return "bg-content-flight";
  }
});
</script>

<template>
  <div
    class="flex p-1 rounded-2xl shadow-md hover:shadow-lg w-full"
    :class="bgTypeColor"
  >
    <div
      class="bg-white flex flex-col sm:flex-row justify-between p-6 rounded-2xl w-full"
    >
      <div class="hidden sm:flex flex-col justify-center items-center ml-4">
        <div
          class="rounded-full w-16 h-16 overflow-hidden p-1 mb-2 flex justify-center items-center"
          :class="bgTypeColor"
        >
          <img
            class="w-full"
            :src="props.trip.announcer.profile_image ?? defaultProfileImage"
            alt=""
          />
        </div>
        <div class="flex text-center">
          <span>
            {{ props.trip.announcer.firstname }}
            {{ props.trip.announcer.lastname }}
          </span>
        </div>
      </div>
      <div class="flex flex-col justify-between sm:w-1/3">
        <div class="py-1">
          <font-awesome-icon class="w-4" icon="fa-flag-checkered" />
          <span class="ml-1">{{ props.trip.departure_location }}</span>
        </div>
        <div class="py-1">
          <font-awesome-icon class="w-4" icon="fa-map-marker-alt" />
          <span class="ml-1">{{ props.trip.arrival_location }}</span>
        </div>
      </div>
      <div class="flex flex-col justify-between sm:w-1/3">
        <div class="py-1 whitespace-nowrap">
          <font-awesome-icon class="w-4" icon="fa-calendar" />
          <span class="ml-1">{{
            DateTime.fromISO(props.trip.departure_time).toLocaleString({
              month: "long",
              day: "2-digit",
              year: "numeric",
              hour: "2-digit",
              minute: "2-digit",
            })
          }}</span>
        </div>
        <div class="py-1">
          <font-awesome-icon class="w-4" icon="fa-users" />
          <span class="ml-1">
            {{
              $t("trip.passenger", {
                count: props.trip.available_seats,
              })
            }}
          </span>
        </div>
      </div>
    </div>
    <div class="!w-14 flex flex-col items-center justify-between mx-4">
      <font-awesome-icon
        :icon="props.trip.type === TripType.DRIVER ? 'fa-car' : 'fa-thumbs-up'"
        size="2xl"
        class="mt-2"
      />
      <Button
        icon="fa-location-arrow"
        iconClass="rotate-45"
        bgColor="content-base"
        class="mb-2"
      />
    </div>
  </div>
</template>
