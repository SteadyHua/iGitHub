<template>
	<view :class="darkMode?'custom-dark':'custom-light'" class="w-h-100">
		<u-navbar :is-back="false" title-color="#ffffff" :background="{background:themeBgColor}">
			<view class="slot-wrap">
				<view class="search-wrap">
					<u-search @search="searchAction" v-model="keyword" :show-action="false"></u-search>
				</view>
			</view>
		</u-navbar>
		<Repos :repos="reposList" />
		<u-loadmore class="padding-bottom padding-top" :status="loadMoreStatus" :load-text="loadText" icon-type="flower" />
	</view>
</template>

<script>
	import { mapGetters } from 'vuex'
	import Repos from '@/pages/repos/repos'
	export default {
		components: {
			Repos
		},
		computed: {
			...mapGetters(['themeBgColor', 'darkMode']),
		},
		data() {
			return {
				loadText: {
					loadmore: this.$t('Empty'),
					loading: this.$t('HardLoading'),
					nomore: this.$t('NoMore')
				},
				loadMoreStatus: 'loadmore',
				reposList: [],
				keyword: '',
				per_page: 30,
				page: 1
			}
		},
		methods: {
			searchAction() {
				this.keyword && this.searchRepos()
			},
			async searchRepos() {
				if (this.$_.isEmpty(this.keyword)) return
				let params = {
					q: this.keyword,
					sort: 'stars',
					order: 'desc',
					per_page: this.per_page,
					page: this.page
				}
				this.loadMoreStatus = 'loading'
				const res = await this.$minApi.searchRepos(params)
				this.loadMoreStatus = 'nomore'
				this.reposList = res.items
			}
		}
	}
</script>
