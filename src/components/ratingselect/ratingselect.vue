<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span class="block positive" @click="select(2, $event)" :class="{'active': selectType === 2}">{{desc.all}}<span
        class="count">{{ratings.length}}</span> </span>
            <span class="block positive" @click="select(0, $event)" :class="{'active': selectType === 0}">{{desc.positive}}<span
        class="count">{{positives.length}}</span></span>
            <span class="block negative" @click="select(1, $event)" :class="{'active': selectType === 1}">{{desc.negative}}<span
        class="count">{{negatives.length}}</span></span>
        </div>
        <div @click="toggleContent" class="switch" :class="{'on':onlyContent}">
            <span class="icon-check_circle"></span>
            <span class="text">只显示有内容的评价</span>
        </div>
    </div>
</template>
<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
export default {
    props: {
        ratings: {
            type: Array,
            /*   */
            default() {
                return [];
            }
        },
        selectType: {
            type: Number,
            default: ALL
        },
        onlyContent: {
            type: Boolean,
            default: false
        },
        desc: {
            type: Object,
            default() {
                return {
                    all: '全部',
                    positive: '满意',
                    negative: '不满意'
                };
            }
        }
    },
    computed: {
        positives() {
            return this.ratings.filter((rating) => {
                return rating.rateType === POSITIVE;
            });
        },
        negatives() {
            return this.ratings.filter((rating) => {
                return rating.rateType === NEGATIVE;
            });
        }
    },
    methods: {
        toggleContent(event) {
            if (!event._constructed) {
                return;
            }
            this.onlyContent = !this.onlyContent;
            this.$emit('increment', 'onlyContent', this.onlyContent);
        },
        select(type, event) {
            if (!event._constructed) {
                return;
            }
            this.selectType = type;
            this.$emit('increment', 'selectType', type);
        },
        needShow(type, text) {
            if (this.onlyContent && !text) {
                return false;
            }
            if (this.selectType === ALL) {
                return true;
            } else {
                return type === this.selectType;
            }
        }
    },
    data() {
        return {

        };
    }
};
</script>
<style lang="stylus" rel="stylessheet/stylus">
@import "../../common/stylus/mixin";
@import "ratingselect.styl";
</style>
