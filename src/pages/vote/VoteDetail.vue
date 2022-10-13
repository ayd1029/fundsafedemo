<template>
  <q-page class="page-default">
    <div class="row justify-center">
      <div class="col-12 q-pb-xs">

        <!-- <div class="row flex flex-center"> -->
        <!-- <div class="row justify justify-between q-pt-sm"> -->
        <div class="row justify justify-between q-pt-sm">
          <div class="row flex flex-center">
            <div class="row flex flex-center">
              <div class="col">
                <table border="0" width="100%" style="margin: 0px 0px 0px 0px;" cellpadding="0" cellspacing="0" align="center">
                  <tr>
                    <td>
                      <div class="text-right q-pl-sm">
                        <q-avatar size="md">
                          <q-icon name="how_to_vote" size="md" />
                        </q-avatar>
                      </div>
                    </td>
                    <td width="100%">
                      <div class="text-h6 q-pl-sm" style="word-break: break-word;">
                        <div v-if="locale === 'ko-KR'">
                          {{ voteVo.title_ko }}
                        </div>
                        <div v-else class="col-12">
                          {{ voteVo.title }}
                        </div>
                      </div>
                    </td>
                  </tr>
                </table>
              </div>
            </div>
          </div>
          <div class="q-pl-sm">
            <div class="row flex flex-center">
              <!-- <div v-if="popupYn !== 'y'"> -->
              <div>
                <q-btn flat round dense icon="arrow_back" color="black" icon-left="true" @click="goBack" />
              </div>
              <div v-if="getUid === voteVo.reg_id" class="col flex flex-center">
                <q-btn flat round dense icon="edit" size="md" color="primary" icon-right="true" @click="goModify" />
              </div>
              <div v-if="getUid === voteVo.reg_id" class="col">
                <q-btn flat round dense icon="delete" size="md" color="primary" icon-right="true" @click="deleteVote" />
              </div>
              <div class="col">
                <q-btn flat round dense icon="share" size="md" color="black" icon-right="true" @click="shareUrlCopy(voteVo.seq)" />
              </div>
              <div class="q-pl-md q-pr-sm">
                <q-select
                  v-if="popupYn === 'y'"
                  v-model="locale"
                  :options="localeOptions"
                  dense
                  borderless
                  emit-value
                  map-options
                  style="width: 55px"
                  @update:model-value="onChangeLocale"
                />
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
    <q-separator />
    <q-tabs
      v-model="tab"
      class="bg-grey-1 shadow-0"
      no-caps
      align="justify"
      inline-label
    >
      <q-tab name="i" icon="how_to_vote">&nbsp;&nbsp;{{ $t('vote') }}</q-tab>
      <q-tab name="l" icon="rocket">&nbsp;&nbsp;{{ $t('project') }}</q-tab>
      <q-tab name="c" icon="chat">&nbsp;&nbsp;{{ $t('comment') }}</q-tab>
    </q-tabs>

    <q-separator />


    <!-- <q-page-scroller position="top" :scroll-offset="150" :offset="[0, 10]">
      <q-btn fab icon="keyboard_arrow_up" color="primary" style="z-index: 9;" class="z-top" />
    </q-page-scroller> -->


    <q-tab-panels v-model="tab">
      <!-- ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ -->
      <!-- 정보 패널 -->
      <!-- ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ -->
      <q-tab-panel name="i">
        <!-- <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('vote_title') }}</span>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div v-if="locale === 'ko-KR'" class="col-12">
            {{ voteVo.title_ko }}
          </div>
          <div v-else class="col-12">
            {{ voteVo.title }}
          </div>
        </div> -->
        <!-- <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('vote_description') }}</span>
            <q-separator />
          </div>
        </div> -->
        <div class="q-pt-sm q-pb-md">
          <div v-if="locale === 'ko-KR'" class="row q-pb-md q-pl-xs">
            <div class="col-12" v-html="voteVo.description_ko" />
          </div>
          <div v-else class="row justify-center q-pb-md q-pl-xs">
            <div class="col-12" v-html="voteVo.description" />
          </div>
        </div>

        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-h6 text-grey-6">{{ $t('vote_withdrawal_request_amount') }}</span>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center text-h6 q-pb-md">
          <div class="col-12">
            {{ voteVo.request_amount.toLocaleString() }}
            <span v-if="projectVo.mainnet === 'KLAYTN'">KLAY</span>
            <span v-if="projectVo.mainnet === 'ETHEREUM'">ETHEREUM</span>
          </div>
        </div>

        <!-- main image -->
        <div v-if="voteVo.main_image" class="q-pt-md q-pb-md q-pl-xs">
          <img :src="voteVo.main_image" style="width: 100%" />
        </div>

        <br />
        <q-separator spaced style="height: 5px;" />
        <br />

        <!-- holder_list -->
        <div class="row justify-center q-pt-md q-pb-md">
          <div class="col-12 text-center">
            <q-btn class="btn" text-color="black" size="lg" style="width: 98%; max-width: 200px;" no-caps @click="showHolderList" outline rounded>
              <b>{{ $t('vote_holder_list') }}</b>
            </q-btn>
          </div>
        </div>

        <!-- progress -->
        <div class="row justify-center q-pt-md">
          <div class="col-9 text-left">
            <span class="text-weight-bold text-subtitle1"><span class="text-green text-bold">{{ $t('vote_agree') }}</span>
            &nbsp;( <span class="text-green text-bold">{{ voteVo.total_agree }}</span> / {{ voteVo.total }} )</span>
          </div>
        </div>
        <div class="row justify-center">
          <div class="col-9">
            <q-linear-progress size="40px" :value="progressAgree" color="green-3" stripe rounded>
              <div class="absolute-full flex flex-center">
                <q-badge color="white" text-color="black">
                  {{ progressAgreeLabel }} %
                </q-badge>
              </div>
            </q-linear-progress>
          </div>
        </div>
        <div class="row justify-center q-pt-lg">
          <div class="col-9 text-left">
            <span class="text-weight-bold text-subtitle1"><span class="text-red text-bold">{{ $t('vote_disagree') }}</span>
            &nbsp;( <span class="text-red text-bold">{{ voteVo.total_disagree }}</span> / {{ voteVo.total }} )</span>
          </div>
        </div>
        <div class="row justify-center">
          <div class="col-9">
            <q-linear-progress size="40px" :value="progressDisagree" color="red-3" stripe rounded>
              <div class="absolute-full flex flex-center">
                <q-badge color="white" text-color="black">
                  {{ progressDisagreeLabel }} %
                </q-badge>
              </div>
            </q-linear-progress>
          </div>
        </div>

        <!-- refresh -->
        <div class="row justify-center q-pt-xl q-pb-md">
          <div class="col-12 text-center">
            <q-btn :loading="loadingRefresh" text-color="black" size="md" @click="refreshVoteRate(true)" icon="refresh" round outline>
              <span v-if="loadingRefresh">
                <q-spinner-oval class="" />
              </span>
            </q-btn>
          </div>
        </div>

        <br />
        <q-separator spaced style="height: 5px;" />
        <br />

        <!-- agree & disagree button -->
        <div class="q-pt-lg q-pb-md q-pl-xs">
          <div class="text-center text-h5">
            {{ $t('confirm_vote_agree') }}
          </div>
        </div>
        <div class="row justify-center q-pt-md">
          <div class="col-6 text-right q-pr-sm">
            <q-btn class="btn" color="green-3" text-color="black" size="lg" style="width: 98%; max-width: 150px;" no-caps @click="vote('Y')">
              <b>{{ $t('vote_agree') }}</b>
            </q-btn>
          </div>
          <div class="col-6 text-left q-pl-sm">
            <q-btn class="btn" color="red-3" text-color="black" size="lg" style="width: 98%; max-width: 150px;" no-caps @click="vote('N')">
              <b>{{ $t('vote_disagree') }}</b>
            </q-btn>
          </div>
          <br /><br /><br /><br /><br /><br /><br />
        </div>

        <!-- place QPageScroller at end of page -->
        <q-page-scroller position="bottom-right" :scroll-offset="150" :offset="[18, 18]">
          <q-btn fab icon="keyboard_arrow_up" color="primary" />
        </q-page-scroller>

      </q-tab-panel>
      <!-- ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ -->
      <!-- 2. 프로젝트 패널 -->
      <!-- ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ -->
      <q-tab-panel name="l" style="word-break: break-word;">
        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_title') }}</span>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div v-if="locale === 'ko-KR'" class="col-12">
            {{ projectVo.title_ko }}
          </div>
          <div v-else class="col-12">
            {{ projectVo.title }}
          </div>
        </div>
        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_summary') }}</span>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div v-if="locale === 'ko-KR'" class="col-12">
            {{ projectVo.summary_ko }}
          </div>
          <div v-else class="col-12">
            {{ projectVo.summary }}
          </div>
        </div>
        <div v-if="projectVo.description || projectVo.description_ko" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_description') }}</span>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.description || projectVo.description_ko" class="q-pb-md q-pl-xs">
          <div v-if="locale === 'ko-KR'" class="row q-pb-md q-pl-xs">
            <div class="col-12" v-html="projectVo.description_ko" />
          </div>
          <div v-else class="row justify-center q-pb-md q-pl-xs">
            <div class="col-12" v-html="projectVo.description" />
          </div>
        </div>

        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('mainnet') }}</span>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            {{ projectVo.mainnet }}
          </div>
        </div>

        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_type') }}</span>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            {{ projectVo.type }}
          </div>
        </div>

        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_wallet_address') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.wallet_address)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(scopeKlaytnUrl + 'account/' + projectVo.wallet_address )" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.wallet_address }}</a>
          </div>
        </div>

        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_token_contract_address') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.token_contract_address)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(scopeKlaytnUrl + 'account/' + projectVo.token_contract_address )" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.token_contract_address }}</a>
          </div>
        </div>

        <div class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_platform_contract_address') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.platform_contract_address)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(scopeKlaytnUrl + 'account/' + projectVo.platform_contract_address )" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.platform_contract_address }}</a>
          </div>
        </div>

        <div v-if="projectVo.official_website" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_official_website') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.official_website)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.official_website" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.official_website)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.official_website }}</a>
          </div>
        </div>
        <div v-if="projectVo.official_email" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_official_email') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.official_email)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.official_email" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a class="text-weight-bold">{{ projectVo.official_email }}</a>
          </div>
        </div>
        <div v-if="projectVo.docs" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_docs') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.docs)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.docs" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.docs)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.docs }}</a>
          </div>
        </div>
        <div v-if="projectVo.blog" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_blog') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.blog)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.blog" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.blog)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.blog }}</a>
          </div>
        </div>
        <div v-if="projectVo.medium" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_medium') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.medium)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.medium" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.medium)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.medium }}</a>
          </div>
        </div>
        <div v-if="projectVo.telegram" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_telegram') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.telegram)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.telegram" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.telegram)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.telegram }}</a>
          </div>
        </div>
        <div v-if="projectVo.twitter" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_twitter') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.twitter)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.twitter" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.twitter)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.twitter }}</a>
          </div>
        </div>
        <div v-if="projectVo.github" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_github') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.github)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.github" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.github)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.github }}</a>
          </div>
        </div>
        <div v-if="projectVo.meta" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_meta') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.meta)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.meta" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.meta)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.meta }}</a>
          </div>
        </div>
        <div v-if="projectVo.discord" class="row justify-center">
          <div class="col-12">
            <span class="text-weight-bold text-subtitle1 text-grey-6">{{ $t('project_discord') }}</span>
            <q-btn icon="content_copy" color="grey-7" size="md" flat @click="copyValue(projectVo.discord)">&nbsp;{{ $t('copy') }}</q-btn>
            <q-separator />
          </div>
        </div>
        <div v-if="projectVo.discord" class="row justify-center q-pb-md q-pl-xs">
          <div class="col-12">
            <a @click="openUrl(projectVo.discord)" class="text-weight-bold" style="cursor: pointer;">{{ projectVo.discord }}</a>
          </div>
        </div>

        <!-- place QPageScroller at end of page -->
        <q-page-scroller position="bottom-right" :scroll-offset="150" :offset="[18, 18]">
          <q-btn fab icon="keyboard_arrow_up" color="primary" />
        </q-page-scroller>
      </q-tab-panel>
      <!-- ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ -->
      <!-- 3. 댓글 패널 ※ infiniteScroll 이상동작으로 comment는 별도로 기술 -->
      <!-- ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ -->
      <!-- <q-tab-panel name="comment">
        <div>
        </div>
      </q-tab-panel> -->
    </q-tab-panels>

    <!-- 댓글 리스트  -->
    <div v-if="tab === 'c'" class="col-12 justify-center q-pa-md">

      <div class="row">
        <div class="col-12">
          <q-input v-model="myContents" type="textarea" :placeholder="$t('enter_the_comment')" rows="2" outlined @keyup="countMyContentsLength" />
        </div>
      </div>

      <div class="row q-pt-sm q-pb-sm">
        <div class="col-6 text-left">
          &nbsp;&nbsp;&nbsp;{{ myContentsLength }} / 300
        </div>
        <div class="col-6 text-right">
          <q-btn size="md" color="black" style="height: 36px;" @click="insertVoteComment" outline>{{ $t('register') }}</q-btn>
        </div>
      </div>

      <!-- 투표 댓글 리스트 -->
      <q-pull-to-refresh @refresh="refresher">
        <q-infinite-scroll @load="loadMore" :offset="100" ref="infiniteScroll">
          <div v-for="item in voteCommentList" :key="item.seq">
            <q-separator />
            <div :style="`padding-left: ${ item.group_layer * 20 }px`" v-if="item.visible_child" :class="`${ item.group_layer === 0 ? 'bg-white' : 'bg-grey-2'}`">
              <div class="row q-pt-md">
                <div class="col-8">
                  <table border="0" cellpadding="0" sellspacing="0" width="100%">
                    <tr>
                      <td rowspan="2" width="60" class="flex-bottom">
                        <q-avatar>
                          <!-- <img src="https://cdn.quasar.dev/img/avatar7.jpg"> -->
                          <img src="https://cdn.quasar.dev/img/boy-avatar.png">
                        </q-avatar>
                      </td>
                      <td><span class="text-body2">{{ item.reg_name }}</span></td>
                    </tr>
                    <tr>
                      <td><span class="text-caption text-grey-7">{{ item.reg_time }}</span></td>
                    </tr>
                  </table>
                </div>
                <div v-if="getUid && getUid === item.reg_id" class="col-4 text-right q-pt-md">
                  <!-- <span style="cursor: pointer;" @click="modifyComment(item)">{{ $t('modify') }}</span> -->
                  <q-btn icon="edit" @click="modifyComment(item)" flat dense />
                  &nbsp;&nbsp;
                  <!-- <span style="cursor: pointer;" @click="deleteComment(item.seq)">{{ $t('delete') }}</span> -->
                  <q-btn icon="delete" @click="deleteComment(item.seq)" flat dense />
                  &nbsp;&nbsp;
                </div>
              </div>
              <div class="row q-pt-sm" style="word-break: break-word;">
                <div class="col-12 text-body1">{{ item.contents }}</div>
              </div>
              <div class="row q-pt-sm q-pb-sm">
                <div class="col-4">
                  <span style="cursor: pointer; text-body2" @click="showReplyInput(item)">{{ $t('register_reply') }}</span>
                  <span v-if="item.group_layer === 0" style="cursor: pointer; text-body2" @click="showReplyInput(item)">&nbsp;({{ item.reply_cnt1 }})</span>
                  <span v-if="item.group_layer === 1" style="cursor: pointer; text-body2" @click="showReplyInput(item)">&nbsp;({{ item.reply_cnt2 }})</span>
                  <span v-if="item.group_layer === 2" style="cursor: pointer; text-body2" @click="showReplyInput(item)">&nbsp;({{ item.reply_cnt3 }})</span>
                  <span v-if="item.group_layer === 3" style="cursor: pointer; text-body2" @click="showReplyInput(item)">&nbsp;({{ item.reply_cnt4 }})</span>
                  <span v-if="item.group_layer === 4" style="cursor: pointer; text-body2" @click="showReplyInput(item)">&nbsp;({{ item.reply_cnt5 }})</span>
                  <span v-else></span>
                </div>
                <!-- <div class="col-4"></div> -->
                <div class="col-8 text-right">
                  <q-btn v-if="item.like_cd === 'Y'" icon="thumb_up_alt" @click="likeIt(item, 'YES')" flat dense size="sm" />
                  <q-btn v-else icon="thumb_up_off_alt" @click="likeIt(item, 'YES')" flat dense size="sm" />
                  &nbsp;{{ item.like_cnt }}
                  &nbsp;&nbsp;
                  <q-btn v-if="item.like_cd === 'N'" icon="thumb_down_alt" @click="likeIt(item, 'NO')" flat dense size="sm" />
                  <q-btn v-else icon="thumb_down_off_alt" @click="likeIt(item, 'NO')" flat dense size="sm" />
                  &nbsp;{{ item.dislike_cnt }}
                  &nbsp;&nbsp;
                  <!-- ### {{ item.group_order }} ### {{ item.group_layer }} -->
                </div>
              </div>

              <div v-if="item.visible_reply_input" class="row q-pt-sm q-pb-sm">
                <div class="col-12">
                  <q-input v-model="myReply" type="textarea" :placeholder="$t('enter_the_reply')" rows="2" outlined @keyup="countMyReplyLength" />
                </div>

                <div class="col-6 text-left q-pt-sm">
                  &nbsp;&nbsp;&nbsp;{{ myReplyLength }} / 300
                </div>
                <div class="col-6 text-right q-pt-sm">
                  <q-btn size="md" color="black" style="height: 36px;" @click="insertVoteCommentReply(item)" outline>{{ $t('register') }}</q-btn>
                </div>
              </div>

            </div>

          </div>
          <q-separator />
          <br>
          <br>
          <template v-slot:loading>
            <div class="row justify-center q-my-md">
              <q-spinner-dots color="primary" size="40px" />
            </div>
          </template>
        </q-infinite-scroll>
      </q-pull-to-refresh>

      <!-- place QPageScroller at end of page -->
      <q-page-scroller position="bottom-right" :scroll-offset="150" :offset="[18, 18]">
        <q-btn fab icon="keyboard_arrow_up" color="primary" />
      </q-page-scroller>

    </div>

    <div v-if="noDataFlag && tab === 'c'" class="row justify-center">
      <img src="images/sorry-no-data.png" style="width: 50%; max-width: 400px;" />
    </div>

    <!-- 하단 공간 확보 -->
    <div class="row justify-center q-pa-xl">
    </div>

  </q-page>

  <q-dialog v-model="confirmDeleteCommentModal">
    <q-card>
      <q-card-section class="row items-center" style="min-width: 200px;">
        <q-icon name="warning" color="primary" size="md" />
        <span class="q-ml-sm">{{ $t('confirm_delete') }}</span>
      </q-card-section>
      <q-separator />
      <q-card-actions align="around">
        <q-btn flat style="width: 45%;" :label="$t('cancel')" color="black" v-close-popup />
        <q-btn flat style="width: 45%;" :label="$t('delete')" color="black" v-close-popup @click="doDeleteCommennt" />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <q-dialog v-model="confirmModifyCommentModal">
    <q-card>
      <q-card-section class="row items-center" style="min-width: 200px;">
        <q-input v-model="modifyCommentValue" type="textarea" :placeholder="$t('enter_the_comment')" rows="7" outlined />
      </q-card-section>
      <q-separator />
      <q-card-actions align="around">
        <q-btn flat style="width: 45%;" :label="$t('cancel')" color="black" v-close-popup />
        <q-btn flat style="width: 45%;" :label="$t('modify')" color="black" v-close-popup @click="doModifyCommennt" />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <q-dialog v-model="confirmVote">
    <q-card>
      <q-card-section class="row items-center" style="min-width: 200px;">
        <!-- <q-avatar icon="warning" color="primary" text-color="white" size="sm" /> -->
        <q-icon name="warning" color="primary" size="md" />
        <span v-if="agreeYn === 'Y'" class="q-ml-sm">{{ $t('confirm_vote_agree') }}</span>
        <span v-else class="q-ml-sm">{{ $t('confirm_vote_disagree') }}</span>
      </q-card-section>
      <q-separator />
      <q-card-actions align="around">
        <q-btn flat style="width: 45%;" :label="$t('cancel')" color="black" v-close-popup />
        <q-btn v-if="agreeYn === 'Y'" flat style="width: 45%;" :label="$t('vote_agree')" color="black" v-close-popup @click="doVote" />
        <q-btn v-else flat style="width: 45%;" :label="$t('vote_disagree')" color="black" v-close-popup @click="doVote" />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <q-dialog v-model="confirmDeleteVote">
    <q-card>
      <q-card-section class="row items-center" style="min-width: 200px;">
        <!-- <q-avatar icon="warning" color="primary" text-color="white" size="sm" /> -->
        <q-icon name="warning" color="primary" size="md" />
        <span class="q-ml-sm">{{ $t('confirm_delete') }}</span>
      </q-card-section>
      <q-separator />
      <q-card-actions align="around">
        <q-btn flat style="width: 45%;" :label="$t('cancel')" color="black" v-close-popup />
        <q-btn flat style="width: 45%;" :label="$t('delete')" color="black" v-close-popup @click="doDeleteVote" />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <WalletModal ref="refWalletModal" />
  <IframeModal ref="refIframeModal" />
  <HolderListModal ref="refHolderListModal" />

</template>

<script>
import { defineComponent } from 'vue'
import { useI18n } from 'vue-i18n'
// import { useMeta } from 'quasar'
// import { createMetaMixin } from 'quasar'

export default defineComponent({
  setup () {
    const { locale } = useI18n({ useScope: 'global' })
    return {
      locale,
      localeOptions: [
        { value: 'en-US', label: 'EN' },
        { value: 'ko-KR', label: 'KO' },
      ],
    }
  },
  name: 'VoteDetail',
  data () {
    return {
      loadingRefresh: false, // 리프레시 처리시 로딩표시
      agreeYn: '', // 찬성 여부 - Y:찬성 N:반대
      confirmVote: false,
      popupYn: 'n', // 투표정보 참조시 팝업표시 여부 파라미터
      voteSeq: '', // route parameter seq
      tab: 'i',
      voteVo: {
        seq: '',
        // wallet_address: '',
        // platform_contract_address: '',
        title: '',
        title_ko: '',
        description: '',
        description_ko: '',
        main_image: '',
        request_amount: '',
        start_time: '',
        end_time: '',
        fixed_time: '',
        total: 0,
        total_agree: 0,
        total_disagree: 0,
        rate_agree: 0,
        rate_disagree: 0,
        reg_id: '',
      },
      projectVo: {
        seq: '',
        mainnet: '',
        type: '',
        wallet_address: '',
        token_contract_address: '',
        platform_contract_address: '',
        title: '',
        title_ko: '',
        summary: '',
        summary_ko: '',
        description: '',
        description_ko: '',
        official_website: '',
        official_email: '',
        logo_image: '',
        docs: '',
        blog: '',
        medium: '',
        telegram: '',
        twitter: '',
        github: '',
        meta: '',
        discord: '',
        reg_id: '',
      },
      progressAgree: 0.00,
      progressAgreeLabel: '0',
      progressDisagree: 0.00,
      progressDisagreeLabel: '0',
      scopeKlaytnUrl: this.$scopeDomainKlaytn,
      myContents: '', // 입력 댓글
      myContentsLength: 0,
      myReply: '', // 입력 답글
      myReplyLength: 0,
      refresherDone: '',
      pageSize: 50,
      lastPageNum: 1, // 마지막 페이지
      voteCommentList: [],
      noDataFlag: false,
      confirmDeleteCommentModal: false, // 나의 댓글 삭제 모달
      deleteTargetSeq: '', // 삭제 대상 댓글 seq
      confirmModifyCommentModal: false, // 나의 댓글 수정 모달
      modifyTargetSeq: '', // 수정 대상 댓글 seq
      modifyCommentValue: '', // 수정 대상 댓글 내용
      confirmDeleteVote: false, // 투표 삭제 모달
    }
  },
  components: {
  },
  computed: {
    getUid () {
      return this.$store.getters.getUid
    },
    getWalletType () {
      return this.$store.getters.getWalletType
    },
    getWalletAddress () {
      return this.$store.getters.getWalletAddress
    },
  },
  created: function () {
    // 탭 파라미터 존재시, 해당 탭 표시
    if (this.$route.query.t === 'i' || this.$route.query.t === 'l' || this.$route.query.t === 'c') {
      this.tab = this.$route.query.t
    } else {
      this.tab = 'i'
    }
    // locale 파라미터 존재시, 해당 locale 설정
    if (this.$route.query.l === 'ko') {
      this.locale = 'ko-KR'
    } else if (this.$route.query.l === 'en') {
      this.locale = 'en-US'
    } else {
      // klaystar default locale is... ko?
    }
    // popup 파라미터 존재시, popupYn 설정
    if (this.$route.query.p) {
      this.popupYn = this.$route.query.p
    }


    // 키 설정
    this.voteSeq = this.$route.query.s

    // 투표 정보 조회
    this.selectVote()

    // 댓글 건수 조회
    this.selectListMax()
  },
  mounted: function () {},
  methods: {
    callbackLogin(userVo) {
      // console.log('callbackLogin!!!')
      this.$store.dispatch('setUid', userVo.uid)
      this.$store.dispatch('setAdcd', userVo.adcd)
      this.$store.dispatch('setName', userVo.name)
      this.$store.dispatch('setNickname', userVo.nickname)
      this.$store.dispatch('setProfileImage', userVo.profile_image)
      this.$store.dispatch('setWalletType', userVo.wallet_type)
      this.$store.dispatch('setWalletAddress', userVo.wallet_address)
      this.$store.dispatch('setMobileNo', userVo.mobile_no)
    },
    // 답글 글자수 카운트
    countMyReplyLength() {
      this.myReplyLength = this.myReply.length
    },
    // 답글 등록
    insertVoteCommentReply(item) {
      // console.log('insertVoteComment')

      // 로그인 여부 체크, 로그인 모달 표시
      if (!this.getUid) {
        this.$refs.refWalletModal.show()
        return
      }

      // 내용 유무 체크
      if (!this.myReply) {
        this.$noti(this.$q, this.$t('enter_the_reply'))
        return
      }
      
      // 글자수 체크
      if (this.myReply.length > 300) {
        this.$noti(this.$q, this.$t('validation_failed_comment_max_length'))
        return
      }

      this.$q.loading.show() // 로딩 표시 시작
      const params = {
        uid: this.getUid,
        vote_seq: this.voteSeq,
        seq_parent1: item.seq_parent1,
        seq_parent2: item.seq_parent2,
        seq_parent3: item.seq_parent3,
        seq_parent4: item.seq_parent4,
        seq_parent5: item.seq_parent5,
        group_order: item.group_order, // 답글의 대상 값을 넘겨서 이 값보다 큰 GROUP_ORDER 들은 + 1 씩 UPDATE 처리
        group_layer: item.group_layer + 1,
        contents: this.myReply,
      }
      this.$axios.post('/api/votecomment/insertVoteCommentReply', params)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          this.$q.loading.hide() // 로딩 표시 종료
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // console.log(result.data)
            this.myReply = ''
            this.$noti(this.$q, this.$t('register_comment_success'))

            // 목록 새로고침
            this.refresher(null)
          } else {
            this.$noti(this.$q, this.$t('register_comment_failed'))
          }
        })
        .catch((err) => {
          this.$q.loading.hide() // 로딩 표시 종료
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    // 댓글 글자수 카운트
    countMyContentsLength() {
      this.myContentsLength = this.myContents.length
    },
    // 댓글 등록
    insertVoteComment() {
      // console.log('insertVoteComment')

      // 로그인 여부 체크, 로그인 모달 표시
      if (!this.getUid) {
        this.$refs.refWalletModal.show()
        return
      }

      // 내용 유무 체크
      if (!this.myContents) {
        this.$noti(this.$q, this.$t('enter_the_comment'))
        return
      }
      
      if (this.myContents.length > 300) {
        this.$noti(this.$q, this.$t('validation_failed_comment_max_length'))
        return
      }

      this.$q.loading.show() // 로딩 표시 시작

      // seq_parent는 WAS 쿼리에 selectKey로 설정됨
      const params = {
        uid: this.getUid,
        vote_seq: this.voteSeq,
        contents: this.myContents,
      }
      this.$axios.post('/api/votecomment/insertVoteComment', params)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          this.$q.loading.hide() // 로딩 표시 종료
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // console.log(result.data)
            this.myContents = ''
            this.$noti(this.$q, this.$t('register_comment_success'))

            // 목록 새로고침
            this.refresher(null)
          } else {
            this.$noti(this.$q, this.$t('register_comment_failed'))
          }
        })
        .catch((err) => {
          this.$q.loading.hide() // 로딩 표시 종료
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    refresher (done) {
      this.selectListMax()
      // done - Function to call when you made all necessary updates.
      //        DO NOT forget to call it otherwise the refresh message
      //        will continue to be displayed
      // make some Ajax call then call done()
      this.voteCommentList = []
      this.refresherDone = done // load가 끝나면 로딩메세지 종료
      this.$refs.infiniteScroll.reset() // index 초기화
      this.$refs.infiniteScroll.resume() // stop에서 다시 재생
      // this.$refs.infiniteScroll.load() // loadMore로 검색
      this.loadMore(1, done)
    },
    loadMore(index, done) {
      // index - called for nth time
      // done - Function to call when you made all necessary updates.
      //        DO NOT forget to call it otherwise your loading message
      //        will continue to be displayed. Has optional boolean
      //        parameter that invokes stop() when true
      // console.log('index: ' + index)
      // make some Ajax call then call done()
      // this.pageNum = index
      setTimeout(() => {
        // alert(index)
        // console.log('loadMore called index: ' + index)
        if (index <= this.lastPageNum) {
          this.selectList(index, done)
          if (index === this.lastPageNum) {
            this.$refs.infiniteScroll.stop()
          }

          // refresher 로딩메세지 처리
          if (this.refresherDone != null && this.refresherDone !== '') {
            this.refresherDone() // 로딩메세지 종료
            this.refresherDone = '' // 로딩메세지 초기화
          }
        }
      }, 500)
    },
    // 투표 댓글 리스트 마지막 페이지 조회
    selectListMax() {
      this.$axios.get('/api/votecomment/selectVoteCommentListLastPageNum',
        {params: {uid: this.getUid, voteSeq: this.voteSeq, pageSize: this.pageSize}})
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          this.lastPageNum = result.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
    // 투표 댓글 리스트 조회
    selectList(idx, done) {
      this.$axios.get('/api/votecomment/selectVoteCommentList',
        {params: {uid: this.getUid, voteSeq: this.voteSeq, pageNum: idx, pageSize: this.pageSize}})
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          if (idx === 1) { // 첫번째 load인 경우
            this.voteCommentList = [] // 리스트 초기화
          }
          this.voteCommentList = this.voteCommentList.concat(result.data)

          // 데이터 없음 표시 설정
          if (!this.voteCommentList || this.voteCommentList.length < 1) {
            this.noDataFlag = true
          } else {
            this.noDataFlag = false
          }
          if (done) {
            done()
          }
        })
        .catch((err) => {
          console.log(err)
          if (done) {
            done()
          }
        })
    },
    // 투표 조회
    selectVote() {
      const param = {
        uid: this.getUid,
        seq: this.voteSeq,
      }
      this.$axios.get('/api/vote/selectVote', { params: { ...param }})
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          if (result.data) {
            // console.log(result.data)
            this.voteVo = result.data

            // 투표율 설정
            this.progressAgree = result.data.rate_agree / 100
            this.progressAgreeLabel = result.data.rate_agree.toFixed(3)
            this.progressDisagree = result.data.rate_disagree / 100
            this.progressDisagreeLabel = result.data.rate_disagree.toFixed(3)

            // 프로젝트 정보 조회
            this.selectProject(this.voteVo.project_seq)
          } else {
            this.$noti(this.$q, this.$t('request_data_failed'))
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },
    // 프로젝트 조회
    selectProject(project_seq) {
      const param = {
        uid: this.getUid,
        seq: project_seq,
      }
      this.$axios.get('/api/project/selectProject', { params: { ...param }})
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          if (result.data) {
            // console.log(result.data)
            this.projectVo = result.data
          } else {
            this.$noti(this.$q, this.$t('request_data_failed'))
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },
    openUrl(url) {
      // console.log(url)
      // openURL(url)
      // window.open(url, '_system', 'location=yes,footer=yes')

      // cordova인 경우 IframeModal 표시
      if (this.$q.platform.is.cordova || this.$q.platform.is.name === 'webkit') {
        this.$refs.refIframeModal.url = url
        this.$refs.refIframeModal.show()
      } else {
        window.open(url, '_system')
      }
    },
    shareUrlCopy(value) {
      // console.log('copyAddress function called!')
      if (!value) {
        this.$noti(this.$q, 'Value is Empty')
        return
      }
      // API 서버로 보내서 og 정보 달고 다시 frontPath로 넘어옴. path: https://klaystar.com/#/info?seq=999&tab=XXX
      const host = location.protocol + '//' + location.host
      const url = host + '/v?s=' + value + '&t=' + this.tab + '&l=' + this.locale.substring(0, 2) // seq, tab, locale

      // 클립보드로 복사하기
      this.$copyText(url).then(this.copyValueSuccess, this.copyValueFail)
    },
    copyValue(value) {
      if (!value) {
        this.$noti(this.$q, 'Value is Empty')
        return
      }
      // 클립보드로 복사하기
      this.$copyText(String(value)).then(this.copyValueSuccess, this.copyValueFail)
    },
    copyValueSuccess(e) {
      // console.log(e)
      this.$noti(this.$q, this.$t('copy_success'))
    },
    copyValueFail(e) {
      // console.log(e)
      this.$noti(this.$q, this.$t('copy_failed'))
    },
    goBack() {
      try {
        this.$router.go(-1)
      } catch(e) {
        this.$router.push('/vote/voteList')
      }
    },
    // 나의 댓글 삭제 모달 표시
    deleteComment(commentSeq) {
      this.deleteTargetSeq = commentSeq
      this.confirmDeleteCommentModal = true  
    },
    // 나의 댓글 삭제
    doDeleteCommennt() {
      // console.log('insertVoteComment')
      this.$q.loading.show() // 로딩 표시 시작
      const params = {
        uid: this.getUid,
        seq: this.deleteTargetSeq,
      }
      this.$axios.post('/api/votecomment/deleteVoteComment', params)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          this.$q.loading.hide() // 로딩 표시 종료
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // console.log(result.data)
            this.deleteTargetSeq = ''
            this.$noti(this.$q, this.$t('delete_comment_success'))

            // 목록 새로고침
            this.refresher(null)
          } else {
            this.$noti(this.$q, this.$t('delete_comment_failed'))
          }
        })
        .catch((err) => {
          this.$q.loading.hide() // 로딩 표시 종료
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    // 나의 댓글 수정 모달 표시
    modifyComment(item) {
      this.modifyTargetSeq = item.seq
      this.modifyCommentValue = item.contents
      this.confirmModifyCommentModal = true  
    },
    // 나의 댓글 수정
    doModifyCommennt() {
      this.$q.loading.show() // 로딩 표시 시작
      const params = {
        uid: this.getUid,
        seq: this.modifyTargetSeq,
        contents: this.modifyCommentValue,
      }
      this.$axios.post('/api/votecomment/updateVoteComment', params)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          this.$q.loading.hide() // 로딩 표시 종료
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // console.log(result.data)
            this.modifyTargetSeq = ''
            this.$noti(this.$q, this.$t('modify_comment_success'))

            // 목록 새로고침
            this.refresher(null)
          } else {
            this.$noti(this.$q, this.$t('modify_comment_failed'))
          }
        })
        .catch((err) => {
          this.$q.loading.hide() // 로딩 표시 종료
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    // 답글 등록
    showReplyInput(item) {
      // console.log(item.visible_reply_input)

      // 1. 입력값 초기화
      this.myReply = ''

      // 2. 열린 입력창 모두 닫음
      for (let i = 0; i < this.voteCommentList.length; i++) {
        const commentItem = this.voteCommentList[i]
        // 현재 아이템 외 전부 닫기
        if (commentItem.seq !== item.seq) {
          commentItem.visible_reply_input = false

          // 해당 child 표시하기
          if (commentItem.seq_parent1 === item.seq_parent1 && item.group_layer === 0) {
            commentItem.visible_child = !item.visible_reply_input
          }
        }
      }

      // 3. 선택한 입력창 표시
      item.visible_reply_input = !item.visible_reply_input
    },

    // 좋아요/싫어요
    likeIt (item, likeCd) {
      // 로그인 여부 체크, 로그인 모달 표시
      if (!this.getUid) {
        this.$refs.refWalletModal.show()
        return
      }

      // like_cd Y:좋아요 N:싫어요 null:중립
      // 1. 화면 조작
      if (likeCd === 'YES') { // 좋아요인 경우
        if (item.like_cd === 'Y') { // 이전상태 좋아요일 경우
          item.like_cd = null // 중립으로 설정
          item.like_cnt = Number(item.like_cnt) - 1
        } else {
          if (item.like_cd === 'N') { // 이전상태 싫어요일 경우
            item.dislike_cnt = Number(item.dislike_cnt) - 1
          }
          item.like_cd = 'Y' // 좋아요로 설정
          item.like_cnt = Number(item.like_cnt) + 1
        }
      } else { // 싫어요인 경우
        if (item.like_cd === 'N') { // 이전상태 싫어요일 경우
          item.like_cd = null // 중립으로 설정
          item.dislike_cnt = Number(item.dislike_cnt) - 1
        } else {
          if (item.like_cd === 'Y') { // 이전상태 좋아요일 경우
            item.like_cnt = Number(item.like_cnt) - 1
          }
          item.like_cd = 'N' // 싫어요로 설정
          item.dislike_cnt = Number(item.dislike_cnt) + 1
        }
      }

      // 2. 좋아요 테이블 저장
      const params = {
        uid: this.getUid,
        vote_comment_seq: item.seq,
        like_cd: item.like_cd,
      }
      this.$axios.post('/api/votecomment/mergeVoteCommentLike', params)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          // this.$q.loading.hide() // 로딩 표시 종료
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // console.log(result.data)
            // this.$noti(this.$q, this.$t('modify_comment_success'))
          } else {
            // this.$noti(this.$q, this.$t('modify_comment_failed'))
          }
        })
        .catch((err) => {
          // this.$q.loading.hide() // 로딩 표시 종료
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    goModify() {
      this.$router.push({ path: '/vote/modifyVote', query: { seq: this.voteVo.seq }})
    },
    deleteVote() {
      // 삭제 확인창 표시
      this.confirmDeleteVote = true
    },
    // 삭제 확인창에서 삭제 버튼 클릭시 - 삭제 처리
    doDeleteVote() {
      this.$q.loading.show() // 로딩 표시 시작
      const param = {
        uid: this.getUid,
        seq: this.voteVo.seq,
      }
      this.$axios.post('/api/vote/deleteVote', param)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          this.$q.loading.hide() // 로딩 표시 종료
          if (result.data && result.data.resultCd === 'SUCCESS') {
            this.$noti(this.$q, this.$t('delete_success'))

            // 돌아가기
            this.$router.go(-1)
          } else {
            this.$noti(this.$q, this.$t('delete_failed'))
            this.$noti(this.$q, result.data.resultMsg)
          }
        })
        .catch((err) => {
          this.$q.loading.hide() // 로딩 표시 종료
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    // 홀더 리스트 모달 표시
    showHolderList() {
      this.$refs.refHolderListModal.voteSeq = this.voteSeq
      this.$refs.refHolderListModal.fixedTime = this.voteVo.fixed_time
      this.$refs.refHolderListModal.show()
    },
    vote(agreeYn) {
      // 로그인 여부 체크, 로그인 모달 표시
      if (!this.getUid || !this.getWalletAddress) {
        this.$refs.refWalletModal.show()
        return
      } 
      // 찬성여부 설정
      this.agreeYn = agreeYn

      // 확인창 표시
      this.confirmVote = true
    },
    doVote() {
      // 투표하기
      const params = {
        uid: this.getUid,
        vote_seq: this.voteSeq,
        address: this.getWalletAddress,
        agree_yn: this.agreeYn,
      }
      this.$axios.post('/api/vote/updateVoteHolder', params)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          // this.$q.loading.hide() // 로딩 표시 종료
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // console.log(result.data)
            if (this.agreeYn === 'Y') {
              this.$noti(this.$q, this.$t('vote_agree_success'))
            } else {
              this.$noti(this.$q, this.$t('vote_disagree_success'))
            }

            // 투표 정보 리프레시
            this.refreshVoteRate(false)
          } else {
            this.$noti(this.$q, this.$t('vote_not_holder'))
          }
        })
        .catch((err) => {
          // this.$q.loading.hide() // 로딩 표시 종료
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    // 투표 정보 리프레시
    refreshVoteRate(showNoti) {

      if (showNoti) {
        this.loadingRefresh = true // 로딩표시 시작
      }

      const param = {
        uid: this.getUid,
        seq: this.voteSeq,
      }
      this.$axios.get('/api/vote/selectVote', { params: { ...param }})
        .then((result) => {
          // console.log(JSON.stringify(result.data))

          this.loadingRefresh = false // 로딩표시 종료

          if (result.data) {
            // 투표율 설정
            this.progressAgree = result.data.rate_agree / 100
            this.progressAgreeLabel = result.data.rate_agree.toFixed(3)
            this.progressDisagree = result.data.rate_disagree / 100
            this.progressDisagreeLabel = result.data.rate_disagree.toFixed(3)

            this.voteVo.total = result.data.total
            this.voteVo.total_agree = result.data.total_agree
            this.voteVo.total_disagree = result.data.total_disagree

            if (showNoti) {
              this.$noti(this.$q, this.$t('refreshed'))
            }
          } else {
            // this.$noti(this.$q, this.$t('request_data_failed'))
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },
  }
})
</script>

<style scoped>
</style>
