<template>

  <div>
    <KGrid
      class="add-padding"
    >
      <KGridItem
        :layout4="{ span: 1 }"
        :layout8="{ span: 1 }"
        :layout12="{ span: 1 }"
      >
        <KIcon
          icon="quiz"
          class="style-icon"
        />
      </KGridItem>

      <KGridItem
        :layout4="{ span: 3 }"
        :layout8="{ span: 7 }"
        :layout12="{ span: 11 }"
      >
        <KTextbox
          ref="title"
          :label="coachString('titleLabel')"
          :autofocus="true"
          :maxlength="100"
        />
      </KGridItem>
    </KGrid>

    <p>{{ $tr('addSectionsDescription') }}</p>

    <hr class="bottom-border">
    <br>


    <KGrid
      class="kgrid-alignment-style"
    >
      <KGridItem
        :layout12="{ span: 6 }"
        :style="noKgridItemPadding"
      >
        <KTabs
          tabsId="coachReportsTabs"
          ariaLabel="Coach reports"
          :tabs="tabs"
        >
          <template>

          </template>
        </KTabs>
      </KGridItem>

      <KGridItem
        :layout12="{ span: 6 }"
        :style="noKgridItemPadding"
      >
        <KButton
          class="float-button"
          appearance="flat-button"
          icon="plus"
        >
          {{ ($tr('addSection')).toUpperCase() }}
        </KButton>
      </KGridItem>

    </KGrid>

    <hr class="bottom-border">
    <div v-if="isQuestionAvailable">
      <KGrid>
        <KGridItem
          :layout12="{ span: 6 }"
        >
          <div class="left-column-alignment-style">
            <div class="align-kcheckbox-style">
              <p>
                <KCheckbox />
              </p>
            </div>

            <div>
              <p>{{ $tr('selectAllLabel') }}</p>
            </div>
          </div>
        </KGridItem>

        <KGridItem
          :layout12="{ span: 6 }"
        >
          <div class="right-alignment-style">
            <KGrid>
              <KGridItem :layout12="{ span: 4 }">
                <button class="icon-container remove-button-style">
                  <KIcon
                    class="reduce-chervon-spacing"
                    icon="chevronDown"
                  />
                  <KIcon
                    class="reduce-chervon-spacing"
                    icon="chevronUp"
                  />
                </button>
              </KGridItem>

              <KGridItem
                :layout12="{ span: 4 }"
              >

                <KIconButton
                  class="icon-size"
                  icon="refresh"
                />
              </KGridItem>

              <KGridItem
                :layout12="{ span: 4 }"
              >
                <KIconButton
                  class="icon-size"
                  icon="trash"
                />
              </KGridItem>
            </KGrid>
          </div>
        </KGridItem>

      </KGrid>
      <DragContainer
        :items="placeholderList"
        @sort="handleOrderChange"
      >
        <AccordionContainer>
          <template
            #default="{ isItemExpanded, toggleItemState, closeAccordionPanel }"
          >
            <Draggable
              v-for="(item,index) in placeholderList"
              :key="item.id"
              tabindex="-1"
            >
              <AccordionItem
                :id="item.id"
                :key="item.id"
                :items="placeholderList"
                :title="item.title"
                :expanded="isItemExpanded(item.id)"
              >
                <template
                  #heading="{ title }"
                  :accordionToggle="onAccordionToggle(item.id)"
                >
                  <DragHandle>
                    <button
                      tabindex="-1"
                      aria-expanded="false"
                      aria-label="toggle-button"
                      class="remove-button-style"
                    >
                      <div
                        class="flex-div"
                      >
                        <div
                          class="left-column-alignment-style"
                        >

                          <button
                            class="remove-button-style"
                            @click="closeAccordionPanel(item.id)"
                          >
                            <DragSortWidget
                              class="drag-icon sort-widget"
                              :moveUpText="$tr('upLabel', { name: item.title })"
                              :moveDownText="$tr('downLabel', { name: item.title })"
                              :isFirst="index === 0"
                              :isLast="index === placeholderList.length - 1"
                              @moveUp="shiftOne(index, -1)"
                              @moveDown="shiftOne(index, +1)"
                            />
                          </button>

                          <div
                            class="check-box-style"
                          >
                            <KCheckbox
                              :aria-label="$tr('checkBoxLabel',{ name: item.title })"
                            />
                          </div>
                        </div>

                        <div class="occupy-remaining-space">
                          <button
                            :id="item.id"
                            :aria-controls="item.id"
                            :aria-expanded="isItemExpanded(item.id)"
                            aria-labelledby="question-title2 question-title-context"
                            class="limit-height remove-button-style"
                            @click="toggleItemState(item.id)"
                          >
                            <KGrid>
                              <KGridItem
                                :layout12="{ span: 6 }"
                              >
                                <div style="margin-top:.5em;">
                                  {{ title }}
                                </div>
                              </KGridItem>

                              <KGridItem
                                :layout12="{ span: 6 }"
                              >
                                <div class="right-alignment-style">
                                  <KIcon
                                    v-if="isItemExpanded(item.id)"
                                    class="icon-size toggle-icon"
                                    icon="chevronUp"
                                  />
                                  <KIcon
                                    v-else
                                    class="icon-size toggle-icon"
                                    icon="chevronRight"
                                  />

                                </div>
                              </KGridItem>
                            </KGrid>
                          </button>
                        </div>
                      </div>
                    </button>
                  </DragHandle>
                </template>

                <template
                  v-if="isItemExpanded(item.id)"
                  #content
                >
                  <div
                    id="sect1"
                    aria-labelledby="accordion1id"
                    class="accordion-detail-container"
                  >
                    <KGrid>
                      <KGridItem :layout12="{ span: 8 }">
                        <button
                          class="remove-button-style text-align-start"
                        >
                          {{ $tr('questionPhrase') }}
                        </button>

                        <button
                          class="remove-button-style text-align-start text-vertical-spacing"
                        >
                          {{ $tr('questionSubtitle') }}
                        </button>
                      </KGridItem>

                      <KGridItem
                        :layout12="{ span: 4 }"
                      >
                        <KIconButton
                          class="float-item-left-style"
                          icon="edit"
                        />
                      </KGridItem>
                    </KGrid>

                    <div class="choose-question question">
                      <p class="space-content">
                        {{ $tr('chooseQuestionLabel') }}
                      </p>
                    </div>

                    <hr class="bottom-border">
                    <KButton
                      style="width:100%;margin-bottom:0.5em"
                      appearance="raised-button"
                      icon="plus"
                    >
                      {{ $tr('addAnswer') }}
                    </KButton>
                    <hr>
                  </div>
                </template>
              </AccordionItem>
            </Draggable>
          </template>
        </AccordionContainer>
      </DragContainer>
    </div>


    <div
      v-else
      class="no-question-layout"
    >

      <div class="question-mark-layout">
        <span class="help-icon-style">?</span>
      </div>

      <p class="no-question-style">
        {{ $tr('noQuestionsLabel') }}
      </p>

      <p>{{ $tr('selectResourceGuide') }}</p>

      <KButton
        primary
        icon="plus"
      >
        {{ $tr('addQuestion') }}
      </KButton>


    </div>

  </div>

</template>


<script>

  import commonCoreStrings from 'kolibri.coreVue.mixins.commonCoreStrings';
  import DragHandle from 'kolibri.coreVue.components.DragHandle';
  import Draggable from 'kolibri.coreVue.components.Draggable';
  import DragContainer from 'kolibri.coreVue.components.DragContainer';
  import DragSortWidget from 'kolibri.coreVue.components.DragSortWidget';
  import commonCoach from '../../common';
  import AccordionContainer from './AccordionContainer.vue';
  import AccordionItem from './AccordionItem.vue';

  export default {
    name: 'CreateQuizSection',
    components: {
      AccordionContainer,
      AccordionItem,
      DragHandle,
      Draggable,
      DragContainer,
      DragSortWidget,
    },
    mixins: [commonCoreStrings, commonCoach],
    data() {
      return {
        tabs: [{ id: '', label: this.$tr('sectionLabel') }],
        isQuestionAvailable: true,
        placeholderList: [
          {
            id: 1,
            title: 'question 1',
            visible: false,
            placeholderAnswers: [
              {
                id: 1,
                option: 'bit',
              },
              {
                id: 2,
                option: 'but',
              },
              {
                id: 3,
                option: 'bite',
              },
              {
                id: 4,
                option: 'bait',
              },
              {
                id: 5,
                option: 'bet',
              },
            ],
          },
          {
            id: 2,
            title: 'question 2',
            visible: false,
            placeholderAnswers: [],
          },
          {
            id: 3,
            title: 'question 3',
            visible: false,
            placeholderAnswers: [],
          },
        ],
      };
    },
    computed: {
      noKgridItemPadding() {
        return {
          paddingLeft: '0em',
          paddingRight: '0em',
        };
      },
    },
    methods: {
      handleOrderChange(event) {
        const reorderedList = event.newArray.map(x => {
          if (x.isPlaceholder) {
            return this.placeholderList.find(item => item.id === x.id);
          }
          return x;
        });
        this.placeholderList = reorderedList;
        localStorage.setItem('reorderedList', JSON.stringify(reorderedList));
        this.$store.dispatch('createSnackbar', this.$tr('successNotification'));
      },
      shiftOne(index, delta) {
        const newArray = [...this.placeholderList];
        const adjacentItem = newArray[index + delta];
        newArray[index + delta] = newArray[index];
        newArray[index] = adjacentItem;

        this.handleOrderChange({ newArray });
      },
    },
    $trs: {
      sectionLabel: {
        message: 'section 1',
        context: 'Indicates the section number created',
      },
      addSection: {
        message: 'add section',
        context: 'Label for adding the number of quiz sections',
      },
      noQuestionsLabel: {
        message: 'There are no questions in this section',
        context: 'Indicates that there is no question in the particular section',
      },
      selectResourceGuide: {
        message: 'To add questions, select resources from the available channels.',
        context: 'Explains a way of adding a question',
      },
      addQuestion: {
        message: 'Add Questions',
        context: 'Button label for adding a new question',
      },
      addSectionsDescription: {
        message: 'Add one or more sections to your quiz, according to your needs',
        context:
          'This message indicates that more than one section can be added when creating a quiz.',
      },
      questionPhrase: {
        message: 'Select the word that has the following vowel sound.',
        context: 'Placholder for the question',
      },
      questionSubtitle: {
        message: ' Short <e>, [e]</e>',
        context: 'Placholder content for the question description',
      },
      chooseQuestionLabel: {
        message: 'Choose 1 answer:',
        context: 'Label to indicate the question to be chosen',
      },
      addAnswer: {
        message: 'Add answer',
        context: 'Button text to indicate that more answers can be added to the question.',
      },
      selectAllLabel: {
        message: 'Select all',
        context: 'Label indicates that all available options can be chosen at once.',
      },
      upLabel: {
        message: 'Move {name} up one',
        context: 'Label to rearrange question order. Not seen on UI.',
      },
      downLabel: {
        message: 'Move {name} down one',
        context: 'Label to rearrange question order. Not seen on UI.',
      },
      checkBoxLabel: {
        message: 'Select {name} question"',
        context: 'Checkbox to select the question',
      },
      successNotification: {
        message: 'Question order saved',
        context: 'Success message shown when the admin re-orders question',
      },
    },
  };

</script>


<style lang="scss"  scoped>

  .style-icon {
    width: 2.5em;
    height: 2.5em;
    margin: 1.5em;
  }

  /deep/ .ui-textbox-label {
    width: 76.5em;
  }

  .no-question-layout {
    width: auto;
    height: 16.5em;
    padding: 2.5em;
    text-align: center;
    background-color: #fafafa;
    border: 1px;
    border-radius: 0.5em;
  }

  .question-mark-layout {
    align-items: center;
    width: 2.5em;
    height: 2.5em;
    margin: auto;
    background-color: #dbc3d4;
  }

  .help-icon-style {
    font-size: 1.5em;
    font-weight: 700;
    color: #996189;
  }

  .add-padding {
    padding-top: 2rem;
  }

  .no-question-style {
    font-weight: bold;
  }

  .float-button {
    float: right;
    background-color: #f5f5f5;
  }

  .bottom-border {
    border: 1px solid #dedede;
  }

  .kgrid-alignment-style {
    padding-right: 1em;
    padding-left: 0;
    margin-bottom: -1.5em;
    text-align: left;
  }

  .left-column-alignment-style {
    display: inline-flex;
    margin-left: 1em;
  }

  .right-alignment-style {
    float: right;
    margin-top: 1em;
  }

  .drag-icon {
    margin-top: -0.5em;
    font-size: 1em;
  }

  .accordion-detail-container {
    margin-left: 3em;
  }

  .float-item-left-style {
    float: right;
    margin-top: 1em;
    margin-right: 0.5em;
  }

  .reduce-chervon-spacing {
    padding: 0;
    margin: 0;
    font-size: 1em;
  }

  .icon-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0;
    margin: 0;
  }

  .choose-question {
    height: 40px;
    margin-top: 0.5em;
    background-color: #fafafa;
    border: 1px solid #dedede;
    border-radius: 2px;
  }

  .space-content {
    margin: 0.5em;
    font-size: 1em;
    font-weight: 700;
  }

  .check-box-style {
    margin-top: 0.5em;
    margin-left: 0.5em;
  }

  .toggle-icon {
    margin: 0.5em;
    font-size: 1em;
  }

  .align-kcheckbox-style {
    margin-left: 3em;
  }

  .remove-button-style {
    width: 100%;
    padding: 0;
    background-color: transparent;
    border: 0;
  }

  .occupy-remaining-space {
    flex-grow: 1;
  }

  .flex-div {
    display: flex;
  }

  .text-align-start {
    text-align: start;
  }

  .text-vertical-spacing {
    margin-top: 0.5em;
  }

  .limit-height {
    margin-top: 0.5em;
    margin-bottom: 0.5em;
  }

</style>
