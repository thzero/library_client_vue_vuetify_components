<template>
	<div
		class="text-center"
	>
		<v-card>
			<v-card-title>
				{{ $t('titles.openSource') }}
			</v-card-title>
			<v-card-text
				class="pt-2"
			>
				<v-layout
					wrap
				>
					<v-flex
						xs12
						lg6
						mb-1
						:pr-2="$vuetify.breakpoint.lgAndUp"
					>
						<v-simple-table
							fixed-header
						>
							<template #default>
								<thead>
									<tr>
										<th class="text-center" colspan="2">
											{{ $t('openSource.client') }}
										</th>
									</tr>
									<tr>
										<th class="text-left">
											{{ $t('openSource.resource') }}
										</th>
										<th class="text-left">
											{{ $t('openSource.license') }}
										</th>
									</tr>
								</thead>
								<tbody>
									<tr
										v-for="(item2, jndex) in dependenciesClient"
										:key="key(jndex, 'b')"
									>
										<td class="text-left">
											<a
												v-if="item2.url"
												class="opensource-padding"
												:href="item2.url"
											>
												{{ item2.name }}
											</a>
											<span
												v-if="!item2.url"
												class="opensource-padding"
											>
												{{ item2.name }}
											</span>
										</td>
										<td class="text-left">
											<a
												v-if="item2.licenseUrl"
												class="opensource-padding"
												:href="item2.licenseUrl"
											>
												{{ item2.licenseName }}
											</a>
											<span
												v-if="!item2.licenseUrl"
												class="opensource-padding"
											>
												{{ item2.licenseName }}
											</span>
										</td>
									</tr>
								</tbody>
							</template>
						</v-simple-table>
					</v-flex>
					<v-flex
						xs12
						lg6
						mb-1
						:pl-2="$vuetify.breakpoint.lgAndUp"
					>
						<v-simple-table
							fixed-header
						>
							<template #default>
								<thead>
									<tr>
										<th class="text-center" colspan="2">
											{{ $t('openSource.server') }}
										</th>
									</tr>
									<tr>
										<th class="text-left">
											{{ $t('openSource.resource') }}
										</th>
										<th class="text-left">
											{{ $t('openSource.license') }}
										</th>
									</tr>
								</thead>
								<tbody>
									<tr
										v-for="(item2, jndex) in dependenciesServer"
										:key="key(jndex, 'b')"
									>
										<td class="text-left">
											<a
												v-if="item2.url"
												class="opensource-padding"
												:href="item2.url"
											>
												{{ item2.name }}
											</a>
											<span
												v-if="!item2.url"
												class="opensource-padding"
											>
												{{ item2.name }}
											</span>
										</td>
										<td class="text-left">
											<a
												v-if="item2.licenseUrl"
												class="opensource-padding"
												:href="item2.licenseUrl"
											>
												{{ item2.licenseName }}
											</a>
											<span
												v-if="!item2.licenseUrl"
												class="opensource-padding"
											>
												{{ item2.licenseName }}
											</span>
										</td>
									</tr>
								</tbody>
							</template>
						</v-simple-table>
					</v-flex>
				</v-layout>
			</v-card-text>
		</v-card>
	</div>
</template>

<script>
import baseOpenSource from '@/library_vue/components/baseOpenSource';

export default {
	name: 'BaseOpenSource',
	extends: baseOpenSource
};
</script>

<style scoped>
	.opensource-padding {
		padding-left: 16px
	}
</style>
