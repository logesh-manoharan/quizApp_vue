<template>
    <div>
        <b-nav tabs>
            <b-nav-item active>Fantasy Quiz</b-nav-item>
            <b-nav-item disabled>Correct: {{numCorrect}}</b-nav-item>
            <b-nav-item disabled>Attempted: {{numTotal}} / 10</b-nav-item>
            <b-nav-item disabled>Score: {{numCorrect}}</b-nav-item>
        </b-nav>
    </div>
</template>


<script>
export default {
    name: "NavigationComponent",
    props: {
        numCorrect: Number,
        numTotal: Number
    }
}
</script>
