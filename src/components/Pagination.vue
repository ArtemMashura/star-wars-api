<template>
    <section>
        <p class="pagination-container">
            <button class="fal fa-chevron-left" @click="changePage(-1)">&lt;-</button>
            <span class="inner-pagination-content">
                Page {{ page }} of {{ pages }}
            </span>
            <button class="fal fa-chevron-right" @click="changePage(1)">-&gt;</button>              
        </p>
    </section>
</template>

<script>
export default {
    props: ['totalRecords'],
    data: function () {
        return {
            page: 1,
            perPage: 10
        }
    },
    computed: {
        pages () {
            const remainder = this.totalRecords % this.perPage

            if (remainder > 0) {
                return Math.floor(this.totalRecords / this.perPage) + 1
            } else {
                return this.totalRecords / this.perPage
            }
        }
    },
    methods: {

        changePage (val) {
            switch (val) {
                case -1:
                    this.page = this.page > 1 ? this.page - 1 : this.page;
                    this.$emit('input', this.page)
                    break;
                case 1:
                    this.page = this.page < this.pages ? this.page + 1 : this.page;
                    this.$emit('input', this.page)
                    break;
            }
        }
    }
}
</script>

<style lang="scss">

.pagination-container {
    display: flex;
    justify-content: flex-end;
    color: #444;
    margin-right: 20px;

    .inner-pagination-content {
        display: flex;
        align-items: center;
        margin: 0px 10px;
    }

    .pagination-seperator, .showing {
        font-size: 16px;
        font-weight: 300;
        color: #888;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0px 10px;
    }

    .showing {
        margin: 0px 5px;
        cursor: pointer;

        &.active {
            color: #2997ff;
        }
    }
}

.fal {
    width: 20px;
    color: #2997ff;
    display: flex;
    align-items: center;
    justify-content: center;
    padding-top: 2.5px;
    cursor: pointer;
}

</style>