<template>
  <div v-if="useNoteState.state.currentNote.noteTitle" id="editor">
    <div class="editor-header">
      <input
        class="title"
        type="text"
        name="noteTitle"
        :value="useNoteState.state.currentNote.noteTitle"
        @input="updateNoteTitle"
      />
      <span
        @click="
          updateNote(
            newNoteTitle,
            newNoteBody,
            category,
            useNoteState.state.currentNote
          )
        "
        ><i class="far fa-save"></i
      ></span>

      <span
        @click="
          deleteNote(
            useNoteState.state.currentNote.noteTitle,
            category.currentCategory
          )
        "
        ><i class="far fa-trash-alt"></i
      ></span>
    </div>
    <textarea
      name="noteBody"
      :value="useNoteState.state.currentNote.noteBody"
      @input="updateNoteBody"
    ></textarea>

    <div class="editor-footer">
      <div class="tags">
        <span
          :key="tag.tagName"
          v-for="tag in useNoteState.state.currentNote.noteTags"
          :style="{
            background: `${useTagState.state.allTags[tag].tagColor} `,
          }"
          @dblclick="
            deleteTagFromNote(
              useTagState.state.allTags[tag].id,
              useNoteState.state.currentNote,
              category.currentCategory
            )
          "
        >
          #{{ useTagState.state.allTags[tag].tagName }}
        </span>
      </div>

      <div class="actions">
        <div></div>
        <select name="tags" v-model="noteTag">
          <option
            :key="tag.tagName"
            v-for="tag in useTagState.state.allTags"
            :style="{
              background: `${tag.tagColor} `,
            }"
          >
            {{ tag.tagName }}
          </option>
        </select>
        <span
          @click="
            addTagToNote(
              noteTag,
              useTagState.state.allTags,
              useNoteState.state.currentNote,
              category.currentCategory
            )
          "
        >
          <i class="far fa-plus-square"></i>
        </span>
        >
      </div>
    </div>
  </div>
</template>

<script>
import { inject, ref } from "vue";
// useTagState.state.allTags[tag]
export default {
  name: "NoteEditor",
  props: ["category"],

  setup() {
    const useNoteState = inject("useNoteState");
    const useTagState = inject("useTagState");
    const { editNote, deleteNote, addTagToNote, deleteTagFromNote } =
      useNoteState;

    const newNoteTitle = ref(null);
    const newNoteBody = ref(null);

    const updateNoteTitle = (e) => (newNoteTitle.value = e.target.value);
    const updateNoteBody = (e) => (newNoteBody.value = e.target.value);

    const updateNote = (noteTitle, noteBody, category, oldNote) => {
      const updatedNote = { noteTitle, noteBody };
      editNote(updatedNote, category.currentCategory, oldNote);

      newNoteTitle.value = null;
      newNoteBody.value = null;
    };

    const noteTag = ref(null);

    return {
      useNoteState,
      useTagState,

      newNoteTitle,
      newNoteBody,
      updateNoteTitle,
      updateNoteBody,

      updateNote,
      deleteNote,
      addTagToNote,
      deleteTagFromNote,

      noteTag,
    };
  },
};
</script>

<style lang="scss">
#editor {
  display: flex;
  flex-direction: column;
  margin: 0;
  height: 100%;
  width: 70%;
  color: #333;
}

.editor-header {
  display: flex;
  background-color: #fafafa;

  input {
    width: 100%;
  }

  span {
    margin: auto .5em;
    font-size: 1.1em;
    cursor: pointer;
  }
}

.title {
  justify-content: left;
  padding-left: 0.5em;
  font-size: 16px;
  background-color: #fafafa;
  font-weight: 600;
  border-radius: 0;
  border-bottom: 1px solid grey;
  outline: none;
}

textarea {
  display: inline-block;
  height: 100%;
  width: 100%;
  vertical-align: top;
  box-sizing: border-box;
  padding: 0 20px;

  &::-webkit-scrollbar {
    width: 0.6em;
  }

  &::-webkit-scrollbar-track {
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  }

  &::-webkit-scrollbar-thumb {
    background-color: darkgrey;
    outline: 1px solid slategrey;
  }
}

textarea {
  border: none;
  border-right: 1px solid #ccc;
  resize: none;
  outline: none;
  background-color: #fafafa;
  font-size: 14px;
  padding: 20px;
}

.editor-footer {
  display: flex;
  justify-content: space-between;

  .tags {
    display: flex;
    flex-wrap: wrap;
    span {
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
      color: #fff;
      margin: 0.5em;
      font-size: 13px;
    }
  }

  .actions {
    display: flex;

    span {
      font-size: 1.2em;
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
      margin: auto 0.5em;
      height: 1em;
      &:hover {
        color: #797979;
      }
    }

    select {
      border-radius: 5px;
      cursor: pointer;
      margin: auto 0;
      height: 2em;
    }
  }
}
</style>