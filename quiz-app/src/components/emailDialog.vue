<template>
  <div class="emailModal">
    <div class="modalWrapper">
      <div class="modalContainer">
        <div class="modalHeader">
          <h3>Message</h3>
          <a type="button" @click="$emit('close')" class="close"><span >&times;</span></a>
        </div>
        <div class="modalBody">
          <div name="body">Email has been successfully sent!!</div>
        </div>
        <div class="modalFooter">
          <slot name="footer">
            <button class="button" @click="$emit('close')">OK</button>
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>