<template>
  <!--begin::Chat drawer-->
  <div
      id="kt_drawer_chat"
      class="bg-white"
      data-kt-drawer="true"
      data-kt-drawer-activate="true"
      data-kt-drawer-close="#kt_drawer_chat_close"
      data-kt-drawer-direction="end"
      data-kt-drawer-name="chat"
      data-kt-drawer-overlay="true"
      data-kt-drawer-toggle="#kt_drawer_chat_toggle"
      data-kt-drawer-width="{default:'300px', 'md': '500px'}"
  >
    <!--begin::Messenger-->
    <div id="kt_drawer_chat_messenger" class="card w-100">
      <!--begin::Card header-->
      <div id="kt_drawer_chat_messenger_header" class="card-header pe-5">
        <!--begin::Title-->
        <div class="card-title">
          <!--begin::User-->
          <div class="d-flex justify-content-center flex-column me-3">
            <a
                class="
                fs-4
                fw-bolder
                text-gray-900 text-hover-primary
                me-1
                mb-2
                lh-1
              "
                href="#"
            >Brian Cox</a
            >

            <!--begin::Info-->
            <div class="mb-0 lh-1">
              <span
                  class="badge badge-success badge-circle w-10px h-10px me-1"
              ></span>
              <span class="fs-7 fw-bold text-gray-400">Active</span>
            </div>
            <!--end::Info-->
          </div>
          <!--end::User-->
        </div>
        <!--end::Title-->

        <!--begin::Card toolbar-->
        <div class="card-toolbar">
          <!--begin::Menu-->
          <div class="me-2">
            <button
                class="btn btn-sm btn-icon btn-active-icon-primary"
                data-kt-menu-flip="top-end"
                data-kt-menu-placement="bottom-end"
                data-kt-menu-trigger="click"
            >
              <i class="bi bi-three-dots fs-3"></i>
            </button>
            <Dropdown4></Dropdown4>
          </div>
          <!--end::Menu-->

          <!--begin::Close-->
          <div
              id="kt_drawer_chat_close"
              class="btn btn-sm btn-icon btn-active-icon-primary"
          >
            <span class="svg-icon svg-icon-2x">
              <inline-svg src="/media/icons/duotune/arrows/arr061.svg"/>
            </span>
          </div>
          <!--end::Close-->
        </div>
        <!--end::Card toolbar-->
      </div>
      <!--end::Card header-->

      <!--begin::Card body-->
      <div id="kt_drawer_chat_messenger_body" class="card-body">
        <!--begin::Messages-->
        <div
            ref="messagesRef"
            class="scroll-y me-n5 pe-5"
            data-kt-element="messages"
            data-kt-scroll="true"
            data-kt-scroll-activate="true"
            data-kt-scroll-dependencies="#kt_drawer_chat_messenger_header, #kt_drawer_chat_messenger_footer"
            data-kt-scroll-height="auto"
            data-kt-scroll-offset="0px"
            data-kt-scroll-wrappers="#kt_drawer_chat_messenger_body"
        >
          <template v-for="(item, index) in messages" :key="index">
            <MessageIn
                v-if="item.type === 'in'"
                ref="messagesInRef"
                :image="item.image"
                :name="item.name"
                :text="item.text"
                :time="item.time"
            ></MessageIn>
            <MessageOut
                v-if="item.type === 'out'"
                ref="messagesOutRef"
                :image="item.image"
                :text="item.text"
                :time="item.time"
            ></MessageOut>
          </template>
        </div>
        <!--end::Messages-->
      </div>
      <!--end::Card body-->

      <!--begin::Card footer-->
      <div id="kt_drawer_chat_messenger_footer" class="card-footer pt-4">
        <!--begin::Input-->
        <input
            v-model="newMessageText"
            class="form-control form-control-flush mb-3"
            data-kt-element="input"
            placeholder="Type a message"
            @keydown.enter="addNewMessage"
        />
        <!--end::Input-->

        <!--begin:Toolbar-->
        <div class="d-flex flex-stack">
          <!--begin::Actions-->
          <div class="d-flex align-items-center me-2">
            <button
                class="btn btn-sm btn-icon btn-active-light-primary me-1"
                data-bs-toggle="tooltip"
                title="Coming soon"
                type="button"
            >
              <i class="bi bi-paperclip fs-3"></i>
            </button>
            <button
                class="btn btn-sm btn-icon btn-active-light-primary me-1"
                data-bs-toggle="tooltip"
                title="Coming soon"
                type="button"
            >
              <i class="bi bi-upload fs-3"></i>
            </button>
          </div>
          <!--end::Actions-->

          <!--begin::Send-->
          <button
              class="btn btn-primary"
              data-kt-element="send"
              type="button"
              @click="addNewMessage"
          >
            Send
          </button>
          <!--end::Send-->
        </div>
        <!--end::Toolbar-->
      </div>
      <!--end::Card footer-->
    </div>
    <!--end::Messenger-->
  </div>
  <!--end::Chat drawer-->
</template>

<script lang="ts">
import {defineComponent, onMounted, ref} from "vue";
import {ScrollComponent} from "@/assets/ts/components/_ScrollComponent";
import MessageIn from "@/components/messenger-parts/MessageIn.vue";
import MessageOut from "@/components/messenger-parts/MessageOut.vue";
import Dropdown4 from "@/components/dropdown/Dropdown4.vue";

interface KTMessage {
  type: string;
  name?: string;
  image: string;
  time: string;
  text: string;
}

export default defineComponent({
  name: "upgrade-to-pro",
  components: {
    MessageIn,
    MessageOut,
    Dropdown4,
  },
  setup() {
    const messagesRef = ref<null | HTMLElement>(null);
    const messagesInRef = ref<null | HTMLElement>(null);
    const messagesOutRef = ref<null | HTMLElement>(null);

    const messages = ref<Array<KTMessage>>([
      {
        type: "in",
        name: "Brian Cox",
        image: "/media/avatars/150-15.jpg",
        time: "5 Hours",
        text: "How likely are you to recommend our company to your friends and family ?",
      },
      {
        type: "out",
        image: "/media/avatars/150-2.jpg",
        time: "2 Hours",
        text: "Hey there, we’re just writing to let you know that you’ve been subscribed to a repository on GitHub.",
      },
      {
        type: "in",
        name: "Brian Cox",
        image: "/media/avatars/150-15.jpg",
        time: "2 Hour",
        text: "Ok, Understood!",
      },
      {
        type: "out",
        image: "/media/avatars/150-2.jpg",
        time: "2 Hours",
        text: "You’ll receive notifications for all issues, pull requests!",
      },
      {
        type: "in",
        name: "Brian Cox",
        image: "/media/avatars/150-15.jpg",
        time: "1 Hour",
        text: "You can unwatch this repository",
      },
      {
        type: "out",
        image: "/media/avatars/150-2.jpg",
        time: "4 mins",
        text: "Most purchased Business courses during this sale!",
      },
      {
        type: "in",
        name: "Brian Cox",
        image: "/media/avatars/150-15.jpg",
        time: "2 mins",
        text: "Company BBQ to celebrate the last quater achievements and goals. Food and drinks provided",
      },
    ]);

    const newMessageText = ref("");

    const addNewMessage = () => {
      if (!newMessageText.value) {
        return;
      }
      messages.value.push({
        type: "out",
        image: "/media/avatars/150-2.jpg",
        time: "Just now",
        text: newMessageText.value,
      });

      setTimeout(() => {
        if (messagesRef.value) {
          messagesRef.value.scrollTop = messagesRef.value.scrollHeight;
        }
      }, 1);

      newMessageText.value = "";
      setTimeout(() => {
        messages.value.push({
          type: "in",
          name: "Ja Morant",
          image: "/media/avatars/150-15.jpg",
          time: "Just now",
          text: "Thank you for your awesome support!",
        });

        setTimeout(() => {
          if (messagesRef.value) {
            messagesRef.value.scrollTop = messagesRef.value.scrollHeight;
          }
        }, 1);
      }, 2000);
    };

    onMounted(() => {
      if (
          !messagesRef.value ||
          !ScrollComponent.getInstance(messagesRef.value)
      ) {
        return;
      }

      ScrollComponent.getInstance(messagesRef.value).update();
    });

    return {
      messages,
      messagesRef,
      newMessageText,
      addNewMessage,
      messagesInRef,
      messagesOutRef,
    };
  },
});
</script>
