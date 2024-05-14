<script>
export default {
    data() {
        return {
            page: 1,
            pages: 0,
            chars: [],
            name: "",
            status: "",
        };
    },
    methods: {
        fetchPages(page = 1, name = "", status = "") {
            let url = `https://rickandmortyapi.com/api/character?page=${page}${name ? `&name=${name}` : ''}${status ? `&status=${status}` : ''}`
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    this.pages = data.info.pages;
                    this.chars = data.results;
                    this.chars.map((char) => {
                        this.setEpisodeName(char.episode[0], char.id)
                    })

                }).catch((e) => { this.chars = []; this.pages = 0 });
        },
        setEpisodeName(url, id) {
            const index = this.chars.findIndex(el => el.id === id)
            fetch(url).then(response => response.json()).then(data => { this.chars[index].episodeName = data.name })
        },
        setPage(value) {
            this.page = value;
            this.fetchPages(this.page, this.name, this.status);
        },
    },
    computed: {
        firstPage() {
            return this.page === 1;
        },
        lastPage() {
            return this.page === this.pages;
        },
        prevPagination() {
            return this.page < 4 ? 2 : this.page > this.pages - 2 ? this.pages - 3 : this.page - 1
        },
        currPagination() {
            return this.page < 4 ? 3 : this.page > this.pages - 2 ? this.pages - 2 : this.page
        },
        nextPagination() {
            return this.page < 4 ? 4 : this.page > this.pages - 2 ? this.pages - 1 : this.page + 1
        },
    },
    mounted() {
        this.fetchPages(this.page);
    }
};
</script>

<template>
    <main>
        <div class="container content">
            <div class="row filter">
                <input v-model="name" type="text" name="filterByName" placeholder="Filter by name...">
                <select v-model="status" name="filterByStatus">
                    <option value="" disabled selected>Filter by status</option>
                    <option value="Alive">Alive</option>
                    <option value="Dead">Dead</option>
                    <option value="Unknown">Unknown</option>
                    <option value="">All</option>
                </select>
                <button class="btn blue center" @click="this.fetchPages(1, this.name, this.status)">Apply filters</button>
            </div>
            <div v-if="chars.length" class="cards-list">
                <div v-for="char of chars" class="card">
                    <div class="card-image" :key='char.id'>
                        <img :src='char.image' :alt="char.name" />
                    </div>
                    <div class="card-content">
                        <span class="card-title">{{ char.name }}</span>
                        <p class="status"
                            :class="[char.status === 'Dead' ? 'red-dot' : char.status === 'Alive' ? 'green-dot' : 'grey-dot']">
                            {{ char.status }} - {{ char.species }}
                        </p>
                        <div class="card-block">
                            <p class="sub-title">Last known location:</p>
                            <p>{{ char.location.name }}</p>
                        </div>
                        <div class="card-block">
                            <p class="sub-title">First seen in:</p>
                            <p>{{ char.episodeName }}</p>
                        </div>


                    </div>
                </div>
            </div>
            <div v-else class="not-found">
                <div class="not-found_text">There is nothing here</div>
            </div>
            <ul v-if="pages > 1" class="pagination center">
                <li :class="[this.firstPage ? 'disabled' : 'waves-effect']"
                    @click="this.setPage(this.page > 1 ? --this.page : 1)">
                    <i class="material-icons">chevron_left</i>
                </li>
                <li class="waves-effect" :class="[firstPage ? 'active blue' : '']" @click="this.setPage(1)">1</li>
                <li v-if="pages > 2" class="waves-effect">...</li>
                <li v-if="pages > 4" class="waves-effect"
                    :class="[this.page === this.prevPagination ? 'active blue' : '']"
                    @click="this.setPage(prevPagination)">
                    {{ this.prevPagination }}
                </li>
                <li v-if="pages > 2" class="waves-effect"
                    :class="[this.page === this.currPagination ? 'active blue' : '']"
                    @click="this.setPage(currPagination)">
                    {{ this.currPagination }}
                </li>
                <li v-if="pages > 4" class="waves-effect"
                    :class="[this.page === this.nextPagination ? 'active blue' : '']"
                    @click="this.setPage(nextPagination)">
                    {{ nextPagination }}
                </li>
                <li v-if="pages > 4" class="waves-effect">...</li>
                <li class="waves-effect" :class="[this.page === this.pages ? 'active blue' : '']"
                    @click="this.setPage(this.pages)">{{ this.pages }}</li>
                <li :class="[this.lastPage ? 'disabled' : 'waves-effect']"
                    @click="this.setPage(this.page < this.pages ? ++this.page : this.pages)">
                    <i class="material-icons">chevron_right</i>
                </li>
            </ul>
        </div>
    </main>
</template>