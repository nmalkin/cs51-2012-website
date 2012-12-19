PAGES = [
    'achievements',
    'assignments',
    'documents',
    'index',
    'labs',
    'lectures',
    'staff',
]
STYLESHEET = 'style/style.less'

csv = require 'csv'
exec = require('child_process').exec
fs = require 'fs'
jade = require 'jade'
less = require 'less'

task 'clean', 'remove build', ->
    exec 'rm -rf build'

task 'prereqs', 'prerequisites', ->
    if not (fs.existsSync 'build')
        fs.mkdirSync 'build'

task 'static', 'copy static files', ->
    invoke 'prereqs'
    exec 'cp -r static/* build'

task 'css', 'build styles', ->
    invoke 'prereqs'
    style = fs.readFileSync STYLESHEET, 'utf8'
    less.render style, (e, css) ->
        fs.writeFileSync 'build/style.css', css

task 'html', 'build pages', ->
    invoke 'prereqs'
    for page in PAGES
        templateFile = "templates/#{page}.jade"
        raw = fs.readFileSync templateFile, 'utf8'

        optionsFile = "var/#{page}.json"
        if fs.existsSync optionsFile
            options = JSON.parse fs.readFileSync optionsFile, 'utf8'
        else
            options = {}

        options.active = page

        compileOptions =
            filename: templateFile
            pretty: true
        template = jade.compile raw, compileOptions

        html = template options
        fs.writeFileSync "build/#{page}.html", html

task 'all', 'build website', ->
    invoke 'static'
    invoke 'css'
    invoke 'html'


task 'assignments', 'make options files for assignments', ->
    assignments = []
    csv()
        .fromPath('data/assignments.csv')
        .transform (data) ->
            assignments.push
                id: data[0]
                name: data[1]
                in: data[2]
                out: data[3]
                available: false
                solution: false
        .on 'end', ->
            output = JSON.stringify {assignments: assignments}, null, 4
            fs.writeFile 'var/assignments.json', output, 'utf8'

task 'lectures', 'make options files for lectures', ->
    lectures = []
    csv()
        .fromPath('data/lectures.csv')
        .transform (data) ->
            lectures.push
                date: data[0]
                id: data[1]
                title: data[2]
                reading: data[3]
                available: false
        .on 'end', ->
            output = JSON.stringify {lectures: lectures}, null, 4
            fs.writeFile 'var/lectures.json', output, 'utf8'

task 'json', 'make options files for lectures and assignments', ->
    invoke 'assignments'
    invoke 'lectures'
